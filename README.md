# Sample-of-my-Java-Code
This is a Sample of the Code I've written in Java. 

/*
 * To change this license header, choose License Headers inProject Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

package kerbal;

import static java.lang.Math.sqrt;
import java.text.DecimalFormat;
import java.util.Scanner;

/**
 *
 * @author Whittaker
 */
public class Kerbal {
  static  double pi = Math.PI;
  
         private static final int PAGE_SIZE = 25;
        private static void clearConsole() {
            for (int i = 0; i < PAGE_SIZE; i++) {
        System.out.println();
            }
        }
  
  public static DecimalFormat df = new DecimalFormat("###,###,###,###.##");

    public static class Planet {
        double sMA;
        double aPo;
        double pRi;
        double oVelo;
        double eRadius;
        double mass;
        double gP;
        double gravity;
        double eVelo;
        double sOI;
        
        Planet(double sMA, double aPo, double pRi, double oVelo, double eRadius, double mass, double gP, double gravity, double eVelo ,double sOI){
         this.sMA = sMA;   
         this.aPo = aPo;
         this.pRi = pRi;
         this.oVelo = oVelo;
         this.eRadius= eRadius;
         this.mass = mass;
         this.gP = gP;
         this.gravity = gravity;
         this.eVelo = eVelo;
         this.sOI = sOI;
         
        }
         void printPlanet(){
            System.out.println("\t\tOrbital Characteristics");
            System.out.println(df.format("Semi-Major Axis: "+this.sMA));
            System.out.println(df.format("Apoapsis: " + this.aPo));
            System.out.println(df.format("Periapsis: "+ this.pRi));
            System.out.println(df.format("Orbital Velocity: "+ this.oVelo));
            System.out.println(df.format("\t\tPhysical Characteristics"));
            System.out.println(df.format("Equatorial Radius: "+this.eRadius));
            System.out.println(df.format("Mass: "+this.mass));
            System.out.println(df.format("Gravitational Paramater: "+this.gP));
            System.out.println(df.format("Gravity"+this.gravity));
            System.out.println(df.format("Escape Velocity: "+this.eVelo));
            System.out.println(df.format("Sphere of Influence: "+this.sOI));
        }
         static double tH(Planet p1, Planet p2){
             double top = Math.pow(p1.sMA + p2.sMA, 3);
             double bottom = 8* 1.1723328e18;
             double nearly = sqrt(top/bottom);
             double done = pi * nearly;
            // System.out.println(top+"\n "+ bottom+"\n "+ nearly+"\n "+ done);
             return done;
         }
         static double pA(Planet p1, Planet p2){
             double first = sqrt(1.1723328e18/p2.sMA);
             double second = tH(p1, p2)/p2.sMA;
             double third = 180/pi ;
             double done = 180-(first*second*third);
             return done;
         }
         static double deltaV1(Planet p1, Planet p2){
             double first = sqrt(1.1723328e18/p1.sMA)*((sqrt(2*p2.sMA/(p1.sMA + p2.sMA)))-1);
             return first;
         }
         static double ejectVelo(Planet p1, Planet p2, double pOrbit){
            // double first = sqrt(pOrbit*(p1.sOI*deltaV1(p1, p2)-2*p1.gP)+2*p1.sOI*p1.gP);
             double top = pOrbit+p1.eRadius*(p1.sOI*(deltaV1(p1, p2)*deltaV1(p1,p2))-(2*p1.gP))+(2*p1.sOI* p1.gP);
             double bottom = pOrbit+p1.eRadius * p1.sOI;
             double done = sqrt(top/bottom);
             return done;
         }
         static double energy(Planet p1, Planet p2,double pOrbit){
             double first = Math.pow(ejectVelo(p1, p2, pOrbit), 2)/2;
             double second = p1.gP/(pOrbit+p1.eRadius);
             double done = first - second;
             return done;
         }
         static double h (Planet p1, Planet p2, double pOrbit){
             double done = (pOrbit +p1.eRadius) * ejectVelo(p1,p2,pOrbit);
             return done;
         }
         static double pAngle(Planet p1, Planet p2, double pOrbit){
             double done = sqrt(1+(2*energy(p1, p2, pOrbit)*Math.pow(h(p1,p2,pOrbit), 2)/Math.pow(p1.gP, 2)));
             return done;
         }
    }   
    public static void main(String[] args) {
        Planet Moho = new Planet(5.263138304e9, 6315765980e9, 4.210510628e9, 12186, 250000, 2.5263617e21, 1.6860938e11, 2.70, 1161.41, 9.646663e6);
        Planet Eve = new Planet(9.832684544e9, 9.931011387e9, 9.734357701e9, 10811, 700000, 1.2244127e23, 8.1717302e12, 16.7, 4831.96, 8.5109365e7);
        Planet Kerbin = new Planet(1.3599840256e10,1.3599840256e10,1.3599840256e10, 9284.5, 600000, 5.2915793e22, 3.5316000e12,9.81, 3431.03,8.4159286e7);
        Planet Duna = new Planet(2.0726155264e10,2.1783189163e10, 1.9669121365e10, 7147, 320000, 4.5154812e21, 3.0136321e11, 2.94, 1372.41, 4.7921949e7);
        //Planet Dres = new Planet();
        //Planet Jool = new Planet();
        //Planet Eeloo = new Planet();
        
        Planet p1 = Kerbin;
        Planet p2 = Duna;
        
        Scanner input1 = new Scanner(System.in);
        Scanner input2 = new Scanner(System.in);
        Scanner input3 = new Scanner(System.in);
        
        System.out.println("Which planet are you leaving?");
        String fstP = input1.nextLine().toLowerCase();
        
        System.out.println("Which planet are you heading to?");
        String sndP = input2.nextLine().toLowerCase();
        
        System.out.println("What is your parking Orbit? (In metres)");
        double pOrbit = input3.nextDouble();
        
        if(fstP.equals("moho")){
             p1 = Moho;
        }
       else if(fstP.equals("eve")){
           p1 = Eve;
       }
        else if(fstP.equals("kerbin")){
            p1 = Kerbin;
        }
        else if(fstP.equals("duna")){
            p1 = Duna;
        }
        else{
            System.out.println("Invalid Planet");
        }
        if(sndP.equals("moho")){
            p2 = Moho;
        }
        else if(sndP.equals("eve")){
            p2 = Eve;
        }
        else if(sndP.equals("kerbin")){
            p2 = Kerbin;
        }
        else if(sndP.equals("duna")){
            p2 = Duna;
        }
       // System.out.println("tH =: "+df.format(Planet.tH(p1, p2)));
        clearConsole();
        System.out.println("The Phase angle is: "+df.format(Planet.pA(p1, p2))+ " degrees");
        System.out.println("The change in Velocity is: "+df.format(Planet.deltaV1(p1, p2))+"m/s");
        System.out.println("The Ejection Velocity is: "+df.format(Planet.ejectVelo(p1, p2, pOrbit)));
        System.out.println("You should begin your burn when you're "+ Planet.pAngle(p1,p2,pOrbit)+" degrees relative to your planet of origin's orbit");
    }
}
