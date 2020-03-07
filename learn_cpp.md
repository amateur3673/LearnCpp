# Learn ``C++``
### 1.Generic programming
To use generic programming in ``C++``, use ``template<typename T>``, T is the generic data type.
```C++
template <typename T>
class Array{
    private:
       T* ptr; //pointer of the array
    public:
       int size;//size of the array
       Array(T arr[],int n); //constructor
       void print();//print the array
};
template <typename T>
Array<T>:: Array(T arr[],int n) //:: for defining the method out of the class
{
    ptr=new T[n];
    size=n;
    for(int i=0;i<n;i++)ptr[i]=arr[i];
}
template <typename T>
void Array<T>:: print(){
    for(int i=0;i<size;i++){
        cout<<ptr[i]<<" ";
    }
}
int main(){
    int arr[5]={1,2,3,4,5};
    Array<int> a(arr,5);
    cout<<a.size<<endl;
    a.print();
    float c[4]={2.3,4.1,6.2,5.5};
    Array<float> b(c,4);
    cout<<b.size<<endl;
    b.print();
}
```
You can also specify 2 or more generic arguments like this example:  
```C++
class ChouPham{
    public:
       T data1;
       U data2;
       ChouPham(T dt1,U dt2){
           data1=dt1;
           data2=dt2;
       }
       void print(){
           cout<<data1<<endl;
           cout<<data2<<endl;
       }
};
int main(){
    ChouPham <int,string> chau(13,"Chou");
    chau.print();
}
```
If class or function has static member, each instance of a template contains its static variable.
### 2. Scope Resolution Operator.
Scope resolution operator in ``C++`` is ``::``. It is used for following purpose:  
**Acess global variable with the same name of local variable:**  
```C++
int x=4;
int main(){
    int x=10;
    cout<<"Local "<<x<<endl;
    cout<<"Global "<<::x<<endl;
    return 0;
}
```
**Define a function outside a class:**  
```C++
class Dog{
    public:
       void make_sound();
};
void Dog:: make_sound(){
    cout<<"Bow_wow"<<endl;
}
int main(){
    Dog dog;
    dog.make_sound();
}
```
**Acesss a static variable:**  
```C++
class Test{
    public:
      static int x;
      void func(int x){
          cout<<"The value of x is "<<x<<endl;
          cout<<"The value of static variable x is "<<Test::x<<endl;
          Test::x++;
      }
};
int Test::x=2;//static variables in a class in C++ must be explicity defined like this
int main(){
    Test test;
    test.func(5);
    cout<<Test::x<<endl;
}
```
**Multiple inheritance:**  
For multiple inheritance, if there's a same variable name in two ancestor, we can use scope resolution to distingush.  
```C++
class A{
    public: 
       int x;
};
class B{
    public:
       int x;
};
class C: public A,public B{
    public:
     C(int x1,int x2){
        A::x=x1;
        B::x=x2;
     }
};
int main()
{
    C c(3,4);
    cout<<c.A::x<<endl;
    cout<<c.B::x<<endl;
}
```
**For namespace:**  
```C++
#include <iostream>
int main(){
    std::cout<<"name space"<<std::endl;
    return 0;
}
```
**Refer to a class inside another class:**  
```C++
class outside{
    public:
      int x;
      class inside{
          public:
             int x;
      };
};
int main(){
    outside out;
    out.x=5;
    cout<<out.x<<endl;
    outside::inside in;
    in.x=6;
    cout<<in.x<<endl;
}
```
### 3.Object Oriented Programming Problem
#### 3.1 Encapsulation in C++.
Access modifier is used for data hiding. There are 3 types of access modifier.  
``public``: All class members declared under ``public`` name are accessed any where.  
``protected``: All class members declared under ``protected`` keywords cannot be accessed in other location, it can only be accessed in the derived ``class`` and the class of those members.  
``private``: All class members declared under ``private`` keywords cannot be accessed anywhere, except the class those members are declared.    
#### 3.2 Inheritance in C++.
Inheritance is the capability to derive properties and characteristics from another class. The class inherits from another class is called sub-class, the inherited class is called base class or super class.  
Syntax:  
```
class subclassname: access_mode base_class{
    //body of subclass
}
```
For example:
```C++
class parent{
    public:
       int x;
       void print_parent(){
           cout<<this->x<<endl;//this is a pointer in C++
       }
};
class child: public parent{
    public:
       int y;
       void print_child(){
           cout<<this->x<<endl;
           cout<<this->y<<endl;
       }
};
int main()
{
    child c;
    c.x=2;//we can access the property of class parent
    c.y=3;
    c.print_parent();
    c.print_child();
}
```
Access mode in inheritance in C++:  
``public``: If we derive a class from public base name, all the public become public in sub-class and protected become protected in sub-class.  
``protected``: All the ``public`` and ``protected`` members in sub-class become ``protected`` members in derived class.  
``private``: All the ``public`` and ``protected`` members become ``private`` in derive class.  
Notice that in inheritance, the sub-class can only inherit the ``protected`` and ``public`` members in sub-class. You cannot access the ``private`` members of sub-class in derived class.  
Types of inheritance:  
Single inheritance: Inheritance from one class to another class. This is the type of inheritance we discuss above.  
Multiple inheritance: A class can inherit from 2 or more sub-class. For example class A inherits from class B and class C.
```C++
class B{
    public:
       int x;
       void printB(){
           cout<<x<<endl;
       }
};
class C{
    public:
       int y;
       void printC(){
           cout<<y<<endl;
       }
};
class A: public B,public C{
    public:
       void printA(){
        cout<<x<<endl;
        cout<<y<<endl;
       }
};
int main(){
    A a;
    a.x=2;
    a.y=3;
    a.printA();
    a.printB();
    a.printC();
    return 0;
}
```
``C++``, as long as ``python`` are programming languages that support multiple. It's one of the thing i hate the most of these languages. When you use multple inheritance, you will face diamond problem, that is if there is a member that in both sub-class B and C and class A inherits from B and C. When we call that member in class A, this can cause confusing for the program because it doesn't know what class's member. To advoid this, in ``C++``, we must specify the class of that member by using scope resolution operator above.  
We can also expand the inheritance, with multple level of inheritance. For example, class B can inherits from class A, class C can inherits from class B.  
By using inheritance, we avoid repeat our code. And it's a way for **polymorphism** that we discuss below.
#### 3.3 Constructor in C++
A constructor is a member of function class that initialize the object. When you use OOP, you must
initialize the object by constructor.  
By default, if you don't specify the constructor, the program will use the default constructor to initialize the object for you. If you want to write constructor, you cannot specify the returning type. Constructor in a class is a special function, it has the same name of the class itself. In the constructor function, you can pass 1 or more parameters in it. For example:
```C++
class coordinate{
    public:
      float x;
      float y;
      coordinate(float x_coor,float y_coor){
          //constructor
          x=x_coor;
          y=y_coor;
      }
      float length(){
          return sqrt(x*x+y*y);
      }
};
int main()
{
    coordinate coor(2.5,3.7);//initialize the object
    cout<<coor.length();
}
```
If you write the constructor for a class in C++, then when you initialize an object, you must use the constructor to initialize it. What i mean in the example above, you cannot write ``coordinate coor;``. You can implicit or explicit call the constructor:  
- implicit: ``coordinate coor(2.7,3.5)``.
- explicit: ``coordinate coor = coordinate(2.7,3.5)``.  

In addition, you can have more than 1 constructor in C++, they are called: **overloading constructor**.  
**Overloading contructors** obey the rule of the constructor (same name of the class and no returning type). The difference between the inner constructors is that: you pass different parameters into the constructor. And each time the constructor is called, the program will choose the constructor that suited for the paramters. For example:  
```C++
class coordinate{
    public:
      float x;
      float y;
      coordinate(){
          x=0.0;
          y=0.0;
      }
      coordinate(float x_coor,float y_coor){
          //constructor
          x=x_coor;
          y=y_coor;
      }
      float length(){
          return sqrt(x*x+y*y);
      }
};
int main()
{
    coordinate coord1;//call the first constructor
    cout<<coord1.length()<<endl;
    coordinate coord2(1.0,1.0);//call the second constructor
    cout<<coord2.length()<<endl;
}
```
**Copy constructor**: Copy constructor is a function that initialize an object using another object of the same class.
```C++
class coordinate{
    public:
      float x;
      float y;
      coordinate(){
          x=0.0;
          y=0.0;
      }
      coordinate(float x_coor,float y_coor){
          //constructor
          x=x_coor;
          y=y_coor;
      }
      coordinate(coordinate &coord){
          x=coord.x;
          y=coord.y;
      }
      float length(){
          return sqrt(x*x+y*y);
      }
};
int main()
{
    coordinate coord1(2.5,3);
    coordinate coord2=coord1;
    cout<<coord2.x<<endl;
    cout<<coord2.y<<endl;
    return 0;
}
```
In C++, a copy constructor are used in:
- When an object of the class are returned by value.
- When an object are passed into a function.
- When an object are constructed based on another object of the same class.
- When the compiler generates a temporary object.

**Constructor in hierarchy inheritance:**
If class B inherits from class A, then when intialize class B, the contructor of class A must be called first to initialize the member of class A, then it will do the rest of the contructor function of class B. You if you write constructor function for class B, you must write the constructor function for class A. For example:
```C++
class A{
    public:
      int x;
      int y;
    A(int x_data,int y_data){
        x=x_data;
        y=y_data;
    }
};
class B: public A{
    public:
       int z;
       B(int x_data,int y_data,int z_data):A(x_data,y_data){
           //call the constructor of A first, then construct the rest of B
           z=z_data;
       }
       void print(){
           cout<<x<<endl;
           cout<<y<<endl;
           cout<<z<<endl;
       }
};
int main()
{
    B b(2,1,5);
    b.print();
}
```
When you use multiple inheritance, for example: 
```class C: public A,public B```, then constructor of A will be called before B.
#### 3.4 Destructor in C++
Opposite to the constructors, destructors are used for removing object. A destructor are called when objects are out of scope:
- The function ends.
- The program ends.
- Block containing the variable end.
- Delete operator is called.

```C++
class A{
    public:
      int x;
      A(){
          cout<<"Initialize object"<<endl;
      }
      ~A(){ //use ~ to specify the destructor
          cout<<"Delete object"<<endl;
      }
};
int main()
{
    A a;//construct the object
    return 0;
}
```
#### 3.4 Polymorphism in C++
Polymorphism means that have many forms.  
Polymorphism often occurs in the hierarchy inheritance. If class B,C are inherits class A, then, we can use the type of the sub-class to represent the sub-class.
```C++
class Animal{
    public:
      void make_sound(){
          cout<<"Ukm"<<endl;
      }
};
class Dog: public Animal{
    public:
      void make_sound(){
          //overwritting the method in super-class
          cout<<"Bow wow"<<endl;
      }
};
class Cat: public Animal{
    public:
      void make_sound(){
          //overwriting the method in super-class
          cout<<"Meo"<<endl;
      }
};
int main(){
    //Use the pointer of the supper class to refer to the sub-class
    Animal* ptr;
    Animal a;
    Dog d;
    Cat c;
    ptr=&a;
    ptr->make_sound();
    ptr=&d;
    ptr->make_sound();
    ptr=&c;
    ptr->make_sound();
    return 0;
}
```
When the code is executed, it will only print the method of super class. This is called **static resolution** of the function call, the function called are fixed before the program is executed. If you want to run the code properly, use ``virtual`` keyword. In the supper class, replace by: ``virtual void make_sound()``  
**Polimorphism** can be achived by function overloading, the method with the same name in a class or function with the same name, but different paramters.
#### 3.5 Abstraction in C++
This is the most confused when i study ``C++``(including polymorphism). I study OOP in ``Java`` before study ``C++`` and i thought the idea of ``Java`` is clear, easy to understand and doesn't make confusion like ``C++``.  
In practice, there are many class or methods that you cannot explicit implement. For example, in the program about animal, when you build a class Animal, you don't want to anything to be initialize as animal object (because you don't know what is animal, the term animal is so general, and what animal make sound). This leads to Animal is an abstract class.  
We use pure virtual function, in this function we don't implement anythings, and we just declare it.
```C++
class Animal{
    public:
      virtual void make_sound()=0;
};
class Dog: public Animal{
    public:
      void make_sound(){
          //overwritting the method in super-class
          cout<<"Bow wow"<<endl;
      }
};
class Cat: public Animal{
    public:
      void make_sound(){
          //overwriting the method in super-class
          cout<<"Meo"<<endl;
      }
};
int main(){
    //Use the pointer of the supper class to refer to the sub-class
    Animal* ptr;
    Dog d;
    Cat c;
    ptr=&d;
    ptr->make_sound();
    ptr=&c;
    ptr->make_sound();
    return 0;
}
```
If you change your code like above, then Animal class become an abstract class. Here are some properties of abstract class you need to know: 
- A class is abstract if it has at least one pure virtual function.
- We can have pointers and reference of abstract class type, this is used for polymorphism.
- If we don't override the pure virtual function, then the derived function will become an abstract class.
- Abstract class can have constructor.
### 4. Reference in C++
When a variable is declared as reference, it becomes an alternative name for the existing variable. A variable can be declared as reference by putting ``& `` in the declaration.  
```C++
int main()
{
    int x=20;
    int &ref=x;
    cout<<ref<<endl;//ref is equal to x
    x=30;//chang x leads to change ref
    cout<<ref<<endl;
    ref=40;//change ref leads to change x
    cout<<x<<endl;
    return 0;
}
```
Application:  
- Modify the variable without explit use that variable, this is useful when you change the variable in a function.  
```C++
void swap(int &ref1,int &ref2){
    int temp=ref1;
    ref1=ref2;
    ref2=temp;
}
int main()
{
    int x=20;
    int y=30;
    swap(x,y);
    cout<<x<<endl;
    cout<<y<<endl;
}
```
- Avoid copying large structure: if pass a large object without reference to a function, a new object will be created and waste memory.  
- In for each loop:
  ```C++
  vector<int> a{1,2,3,4};
  for(int &x:a)cout<<x<<endl;
  ```

Difference between pointers and reference: both of them are used to change the variable, pass object to a function to avoid copying new object. But there's many differences between them:  
- A pointer can be void type, but you cannot initialize a reference with void type.
- Pointers are way stronger than reference:
- - Once the reference is created, it can not be refer to other objects.
- - Reference cannot be ``NULL``.
- - Reference must be initialize when created.