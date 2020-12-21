package paint;

import basis.*;

public class Paint {

    // Deklaration

    private Fenster fenster;
    private Leinwand leinwand;
    private Stift meinStift, farbStift; 
    private Stift spiegelStift;
    private boolean spiegeln;
    private Maus meineMaus;
    private Tastatur meineTastatur;
    private Knopf kEnde, kLoesche, kHintergrund;
    private TextFeld tfModus;
    private Rollbalken rbRot,  rbGruen , rbBlau;
    private WahlBox wbDueen, wbMittel, wbDick;
    private WahlBoxGruppe wbgLienenbreite ;
    

    // Erzeugung
    public Paint() {
        fenster = new Fenster(800, 600);
        leinwand = new Leinwand(0, 100, fenster.breite() - 130, fenster.hoehe() - 120);
        meinStift = new Stift();
        spiegelStift = new Stift();
        spiegeln = false;
        farbStift = new Stift(leinwand);
        meineMaus = new Maus(leinwand);
        meineTastatur = new Tastatur();
        kEnde = new Knopf("Ende",fenster.breite()-120,fenster.hoehe()-30, 100, 20);
        kLoesche = new Knopf("Löschen",fenster.breite()-120,fenster.hoehe()-60, 100, 20);
        kHintergrund = new Knopf("Hintergrund",fenster.breite()-120,fenster.hoehe()-90, 100, 20);
        rbRot = new Rollbalken(true,fenster.breite()-120,30,100,20);
        rbGruen  = new Rollbalken(true,fenster.breite()-120,60,100,20);  
        rbBlau = new Rollbalken(true,fenster.breite()-120,90,100,20);
        tfModus = new TextFeld(150, 30, 100, 50);
        wbDueen = new WahlBox("dünn",5,30,100,20);
        wbMittel = new WahlBox("mittel",5,50,100,20);
        wbDick = new WahlBox("dick",5,70,100,20);
        wbgLienenbreite  = new WahlBoxGruppe();
        wbgLienenbreite.fuegeEin(wbDick);
        wbgLienenbreite.fuegeEin(wbDueen);
        wbgLienenbreite.fuegeEin(wbMittel);
        wbDueen.setzeZustand(true);
        wbMittel.setzeZustand(false);
        wbDick.setzeZustand(false);
        
        // Benutuzung
        
       
        rbRot.setzeMinimum(0);
        rbRot.setzeMaximum(255);
     
        rbGruen.setzeMinimum(0);
        rbGruen.setzeMaximum(255);
        
        rbBlau.setzeMinimum(0);
        rbBlau.setzeMaximum(255);
        
        leinwand.setzeRand(Farbe.SCHWARZ, 2);
        meinStift.maleAuf(leinwand);
        spiegelStift.maleAuf(leinwand);
        
        

    }

    public void fuehreAus() {
        
        while(!kEnde.wurdeGedrueckt()) {
        	
        Hilfe.kurzePause();
        meinStift.bewegeAuf(meineMaus.hPosition(), meineMaus.vPosition());
        spiegelStift.bewegeBis(meineMaus.vPosition(), meineMaus.hPosition());
        
        //if-else-Verzweigung
        if (meineMaus.istGedrueckt() && spiegeln ) {
			meinStift.runter();
		} else if(meineMaus.istGedrueckt() && !spiegeln){
			meinStift.runter();	
		} else {	
			meinStift.hoch();		
        }
        
        if(kLoesche.wurdeGedrueckt()) {
        leinwand.loescheAlles();
        
        }
        if (rbRot.wurdeBewegt()) {
        	rbRot.setzeWert(rbRot.wert());
        	farbStift.rechteck(10, 20, 30, 70);
        	farbStift.setzeFuellMuster(Muster.GEFUELLT);
        	meinStift.setzeFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
        	farbStift.setzeFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
        }
        if (rbGruen.wurdeBewegt()) {
        	rbGruen.setzeWert(rbGruen.wert());
        	farbStift.rechteck(10, 20, 30, 70);
        	farbStift.setzeFuellMuster(Muster.GEFUELLT);
        	farbStift.setzeFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
        	meinStift.setzeFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
        }
        if(rbBlau.wurdeBewegt()) {
        	rbBlau.setzeWert(rbBlau.wert());
        	farbStift.rechteck(10, 20, 30, 70);
        	farbStift.setzeFuellMuster(Muster.GEFUELLT);
        	farbStift.setzeFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
        	meinStift.setzeFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
        }
//        if(meineTastatur.istGedrueckt('r')) {
//        meinStift.radiere();
//        } else if (meineTastatur.istGedrueckt('n')) {
//        meinStift.normal();
//        } else if (meineTastatur.istGedrueckt('s')) {
//        
//        }
        switch(meineTastatur.zeichen()) {
        case 'r': {
        meinStift.setzeLinienBreite(10);
        meinStift.radiere(); 
        tfModus.setzeText("RADIEREN");
        break;
        
        }
        case 'n': {
        meinStift.setzeLinienBreite(1);
        meinStift.normal();
        tfModus.setzeText("Normal");
        break;
        }
        case 's':{
        
        tfModus.setzeText("SPIEGL");
        break;
        }
        }
        
        
       if(wbDueen.istGewaehlt()) {
    	   meinStift.setzeLinienBreite(0);
       }
        else if(wbMittel.istGewaehlt()) {
       	meinStift.setzeLinienBreite(3);
       }
       else if(wbDick.istGewaehlt()) {
    	meinStift.setzeLinienBreite(5);
       }
       if (kHintergrund.wurdeGedrueckt()) {
    	leinwand.setzeHintergrundFarbe(Farbe.rgb(rbRot.wert(),rbGruen.wert() , rbBlau.wert()));
       }
        }
        
        
        fenster.gibFrei();
        
    
        if(meineMaus.istRechtsGedrueckt()) {
        meinStift.runter(); 
        }
        }


    public static void main(String[] args) {
        Paint paint;
        paint = new Paint();
        paint.fuehreAus();

    }

}
