S -> Single Responsible Principle
O -> Open? Closed Principle
L -> Liskoc Substitution Principle
I -> Interface Segmented Principle
D -> Dependency Inversion Principle

1. A class Should have only 1 reason to change  -> class have only 1 responsiblity to change
2. 

class Marker {
     string name;
     string color;
     int year;
     int price;

     public Marker(String name, String color, int year, int price){
     this.name = name;
     this.color = color;
     this.year = year;
     this.price = price;
     }
}

class invoice{
 private Marker marker;
 private int quantity;

 public invoice(Marker marker , int quantity){
 this.marker = marker;
 this.quantity = quantity;
 }
 public ubt calculatetotal(){
 int price = ((marker.price)*this.quantity);
 return price;
 }
 }

2. If class B is subtype of class A then we should be able to replace object of A with B without breaking the behaviour of program


   Interface segmented Principle
   Interfaces should be such that client should not implement unneccessary functions they do not need

3. Class Should depend on interfaces rather than concrete classes

   
 



 
