### Design Patterns

Design patterns are reusable solutions to common problems that occur during software development. They provide a structured approach to solving design problems and promote code reusability, maintainability, and flexibility. In C++, you can implement various design patterns to improve the structure and organization of your code. Here are some commonly used design patterns in C++:

Depending on the design problem they address, design patterns can be classified in different categories, of which the main categories are:

- Creational Patterns:
They focus on object creation in a flexible and controlled manner. These patterns provide mechanisms for instantiating objects in different ways, hiding the specific details of creation. Some examples of creational patterns include Singleton, Factory Method, Abstract Factory, Builder, and Prototype.
- Structural Patterns:
They focus on the composition of classes and objects to form larger and more flexible structures. These patterns allow you to define relationships between objects and provide ways to modify and extend the existing structure without affecting its individual components. Some examples of structural patterns include Adapter, Decorator, Composite, Proxy, and Bridge.
- Behavioral Patterns:
they focus on the interaction and communication between objects and how behavior is distributed among them. These patterns are used to define algorithms and responsibilities among objects in an organized and flexible manner. Some examples of behavioral patterns include Observer, Strategy, Template Method, Command, Iterator, Visitor and MVC(Model-View-Controller).

##### Creational Patterns:

- Singleton: 
Ensures a class has only one instance and provides global access to it.

```cpp
#include <iostream>

class Singleton {
private:
    // Private constructor to prevent direct instantiation
    Singleton() {}

public:
    // Static method to access the singleton instance
    static Singleton& getInstance() {
        // Create a static instance of the singleton
        static Singleton instance;
        return instance;
    }

    // Public method of the singleton
    void showMessage() {
        std::cout << "Hello from the Singleton!" << std::endl;
    }
};

int main() {
    // Get the singleton instance
    Singleton& singleton = Singleton::getInstance();

    // Call a method on the singleton
    singleton.showMessage();

    return 0;
}
```
In this example, the Singleton class has a private constructor to prevent direct instantiation from outside the class. The only way to obtain an instance of the Singleton class is through the getInstance() static method.

The getInstance() method creates a static instance of the Singleton class and returns it. It ensures that only one instance of the Singleton class is created throughout the program's execution.

In the main() function, we obtain the singleton instance using Singleton::getInstance(). We can then call the showMessage() method on the singleton instance.

When you run the program, it will output:
```cpp
Hello from the Singleton!  
```
The Singleton design pattern ensures that there is only one instance of the class, and it provides a global point of access to that instance. This can be useful in situations where you want to restrict the number of instances of a class and ensure that all parts of the codebase access the same instance.

- Factory Method:
Defines an interface for creating objects, but lets subclasses decide which class to instantiate.
```cpp
#include <iostream>

// Abstract Product
class Product {
public:
    virtual void use() = 0;
};

// Concrete Product A
class ConcreteProductA : public Product {
public:
    void use() override {
        std::cout << "Using ConcreteProductA" << std::endl;
    }
};

// Concrete Product B
class ConcreteProductB : public Product {
public:
    void use() override {
        std::cout << "Using ConcreteProductB" << std::endl;
    }
};

// Creator (Abstract Factory)
class Creator {
public:
    virtual Product* createProduct() = 0;

    void someOperation() {
        // Create a product using the factory method
        Product* product = createProduct();

        // Use the product
        product->use();
    }
};

// Concrete Creator A
class ConcreteCreatorA : public Creator {
public:
    Product* createProduct() override {
        return new ConcreteProductA();
    }
};

// Concrete Creator B
class ConcreteCreatorB : public Creator {
public:
    Product* createProduct() override {
        return new ConcreteProductB();
    }
};

int main() {
    // Create a concrete creator A
    Creator* creatorA = new ConcreteCreatorA();
    creatorA->someOperation();

    // Create a concrete creator B
    Creator* creatorB = new ConcreteCreatorB();
    creatorB->someOperation();

    return 0;
}
```
In this example, the Factory Method design pattern is used to create different types of products (ConcreteProductA and ConcreteProductB) through a common interface (Product).

The Product class is an abstract class that defines the interface for all products. It declares a pure virtual method use() which must be implemented by the concrete product classes.

The Creator class is an abstract class that serves as the factory. It declares the createProduct() method, which is the factory method responsible for creating products. The someOperation() method in the Creator class demonstrates the usage of the factory method. It creates a product using createProduct() and then performs some operations on the product.

The ConcreteCreatorA and ConcreteCreatorB classes are concrete implementations of the creator. They override the createProduct() method to create specific types of products.

In the main() function, we create instances of the concrete creators (ConcreteCreatorA and ConcreteCreatorB). We then call the someOperation() method on each creator, which internally creates the corresponding product using the factory method.
When you run the program, it will output:
```cpp
Using ConcreteProductA
Using ConcreteProductB
```
The Factory Method design pattern allows the client code (main() in this case) to work with the abstract product interface (Product), while deferring the actual creation of the product to the concrete creator classes (ConcreteCreatorA and ConcreteCreatorB). This provides flexibility and allows for easy addition of new product types without modifying the existing client code.

- Abstract Factory:
Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
```cpp
#include <iostream>

// Abstract Product A
class AbstractProductA {
public:
    virtual void use() = 0;
};

// Concrete Product A1
class ConcreteProductA1 : public AbstractProductA {
public:
    void use() override {
        std::cout << "Using ConcreteProductA1" << std::endl;
    }
};

// Concrete Product A2
class ConcreteProductA2 : public AbstractProductA {
public:
    void use() override {
        std::cout << "Using ConcreteProductA2" << std::endl;
    }
};

// Abstract Product B
class AbstractProductB {
public:
    virtual void interact(AbstractProductA* productA) = 0;
};

// Concrete Product B1
class ConcreteProductB1 : public AbstractProductB {
public:
    void interact(AbstractProductA* productA) override {
        std::cout << "Interacting with ConcreteProductB1 and ";
        productA->use();
    }
};

// Concrete Product B2
class ConcreteProductB2 : public AbstractProductB {
public:
    void interact(AbstractProductA* productA) override {
        std::cout << "Interacting with ConcreteProductB2 and ";
        productA->use();
    }
};

// Abstract Factory
class AbstractFactory {
public:
    virtual AbstractProductA* createProductA() = 0;
    virtual AbstractProductB* createProductB() = 0;
};

// Concrete Factory 1
class ConcreteFactory1 : public AbstractFactory {
public:
    AbstractProductA* createProductA() override {
        return new ConcreteProductA1();
    }

    AbstractProductB* createProductB() override {
        return new ConcreteProductB1();
    }
};

// Concrete Factory 2
class ConcreteFactory2 : public AbstractFactory {
public:
    AbstractProductA* createProductA() override {
        return new ConcreteProductA2();
    }

    AbstractProductB* createProductB() override {
        return new ConcreteProductB2();
    }
};

int main() {
    // Create an instance of ConcreteFactory1
    AbstractFactory* factory1 = new ConcreteFactory1();

    // Create products using factory1
    AbstractProductA* productA1 = factory1->createProductA();
    AbstractProductB* productB1 = factory1->createProductB();

    // Interact with the products
    productB1->interact(productA1);

    // Create an instance of ConcreteFactory2
    AbstractFactory* factory2 = new ConcreteFactory2();

    // Create products using factory2
    AbstractProductA* productA2 = factory2->createProductA();
    AbstractProductB* productB2 = factory2->createProductB();

    // Interact with the products
    productB2->interact(productA2);

    return 0;
}
```
In this example, the Abstract Factory design pattern is used to create families of related products (AbstractProductA and AbstractProductB) through a common interface. Each concrete factory (ConcreteFactory1 and ConcreteFactory2) is responsible for creating specific types of products from their respective families.

The AbstractProductA and AbstractProductB classes are abstract product classes that define the interfaces for the products in their respective families. They declare pure virtual methods that must be implemented by the concrete product classes.

The ConcreteProductA1, ConcreteProductA2, ConcreteProductB1, and ConcreteProductB2 classes are concrete product classes that implement the interfaces defined by the abstract product classes.
```cpp
Interacting with ConcreteProductB1 and Using ConcreteProductA1
Interacting with ConcreteProductB2 and Using ConcreteProductA2
```
- Builder:
Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

```cpp
#include <iostream>
#include <string>

// Product
class Pizza {
public:
    void setDough(const std::string& dough) {
        dough_ = dough;
    }

    void setSauce(const std::string& sauce) {
        sauce_ = sauce;
    }

    void setTopping(const std::string& topping) {
        topping_ = topping;
    }

    void showPizza() {
        std::cout << "Pizza with " << dough_ << " dough, " << sauce_ << " sauce, and " << topping_ << " topping." << std::endl;
    }

private:
    std::string dough_;
    std::string sauce_;
    std::string topping_;
};

// Abstract Builder
class PizzaBuilder {
public:
    virtual void buildDough() = 0;
    virtual void buildSauce() = 0;
    virtual void buildTopping() = 0;
    virtual Pizza* getPizza() = 0;
};

// Concrete Builder
class MargheritaPizzaBuilder : public PizzaBuilder {
public:
    MargheritaPizzaBuilder() {
        pizza_ = new Pizza();
    }

    void buildDough() override {
        pizza_->setDough("thin crust");
    }

    void buildSauce() override {
        pizza_->setSauce("tomato");
    }

    void buildTopping() override {
        pizza_->setTopping("mozzarella cheese");
    }

    Pizza* getPizza() override {
        return pizza_;
    }

private:
    Pizza* pizza_;
};

// Director
class PizzaDirector {
public:
    void setPizzaBuilder(PizzaBuilder* builder) {
        pizzaBuilder_ = builder;
    }

    void constructPizza() {
        pizzaBuilder_->buildDough();
        pizzaBuilder_->buildSauce();
        pizzaBuilder_->buildTopping();
    }

private:
    PizzaBuilder* pizzaBuilder_;
};

int main() {
    // Create a MargheritaPizzaBuilder
    MargheritaPizzaBuilder margheritaBuilder;

    // Create a PizzaDirector and set the builder
    PizzaDirector pizzaDirector;
    pizzaDirector.setPizzaBuilder(&margheritaBuilder);

    // Construct the pizza using the director
    pizzaDirector.constructPizza();

    // Get the constructed pizza from the builder
    Pizza* pizza = margheritaBuilder.getPizza();

    // Show the pizza details
    pizza->showPizza();

    // Clean up
    delete pizza;

    return 0;
}
```
In this example, the Builder design pattern is used to construct complex objects step by step (Pizza in this case). The construction process is separated from the actual object creation, allowing different representations of the object to be created using the same construction process.

The Pizza class represents the product that we want to build. It has various attributes such as dough, sauce, and topping. It provides setter methods to set these attributes and a method to display the pizza details.

The PizzaBuilder is an abstract builder class that defines the interface for building a pizza. It declares pure virtual methods for building the dough, sauce, topping, and retrieving the constructed pizza.

The MargheritaPizzaBuilder is a concrete builder class that implements the PizzaBuilder interface. It defines the construction steps for building a Margherita pizza by setting the dough, sauce, and topping of the pizza.

The PizzaDirector class is responsible for managing the construction process. It takes a PizzaBuilder and uses it to construct the pizza step by step.

In the main() function, we create a MargheritaPizzaBuilder and a PizzaDirector.

```cpp
Pizza with thin crust dough, tomato sauce, and mozzarella cheese topping.
```
- Prototype:
Creates new objects by duplicating an existing object, known as a prototype, rather than creating them from scratch.

```cpp
#include <iostream>

class Prototype {
public:
    virtual Prototype* clone() const = 0;
    virtual void print() const = 0;
};

class ConcretePrototype : public Prototype {
private:
    int data;

public:
    ConcretePrototype(int data) : data(data) {}

    Prototype* clone() const {
        return new ConcretePrototype(*this);
    }

    void print() const {
        std::cout << "Data: " << data << std::endl;
    }
};

int main() {
    ConcretePrototype original(100);
    original.print();

    ConcretePrototype* clone = (ConcretePrototype*)original.clone();
    clone->print();

    delete clone;

    return 0;
}
```
In this example, the Prototype class defines the common interface for all prototypes and provides two virtual methods: clone() to clone the object and print() to print its state.

The ConcretePrototype class is a concrete implementation of the prototype. It implements the clone() and print() methods. The clone() method creates a new ConcretePrototype object by copying the current object's state using the copy constructor. The print() method simply prints the data value of the object.

In the main() function, we create an original object of type ConcretePrototype and print it. We then clone the original object by calling its clone() method and save the pointer to the clone. Finally, we print the clone and free the memory using delete.

The Prototype pattern allows us to create new objects by duplicating an existing prototype, which avoids the need to instantiate objects from scratch and makes it easy to create new objects with predefined configurations.
```cpp
Data: 100
Data: 100
```
#### Structural Patterns

- Adapter:
Allows objects with incompatible interfaces to work together.
```cpp
#include <iostream>

// Adaptee: The incompatible class or interface
class Adaptee {
public:
    void specificRequest() {
        std::cout << "Specific Request" << std::endl;
    }
};

// Target: The interface expected by the client
class Target {
public:
    virtual void request() = 0;
};

// Adapter: Adapts the Adaptee to the Target interface
class Adapter : public Target {
private:
    Adaptee* adaptee_;

public:
    Adapter(Adaptee* adaptee) : adaptee_(adaptee) {}

    void request() override {
        adaptee_->specificRequest();
    }
};

int main() {
    // Create an instance of Adaptee
    Adaptee* adaptee = new Adaptee();

    // Create an instance of Adapter, passing the Adaptee object
    Target* adapter = new Adapter(adaptee);

    // Call the request method on the adapter
    adapter->request();

    // Clean up
    delete adapter;
    delete adaptee;

    return 0;
}
```
In this example, the Adapter design pattern is used to adapt an incompatible class or interface (Adaptee) to the target interface (Target) that the client expects to work with.

The Adaptee class represents the existing class or interface with a different interface than what the client expects. It has a specificRequest() method that the client cannot directly use.

The Target class represents the interface that the client expects to interact with. It declares a pure virtual request() method that the client can call.

The Adapter class is the adapter itself. It implements the Target interface and internally holds an instance of Adaptee. It adapts the Adaptee interface to the Target interface by implementing the request() method and delegating the call to the specificRequest() method of the Adaptee.

In the main() function, we create an instance of Adaptee and an instance of Adapter, passing the Adaptee object as a parameter. We then call the request() method on the adapter, which internally calls the specificRequest() method of the Adaptee.

When you run the program, it will output:
```cpp
Specific Request
```
- Decorator:
Dynamically adds responsibilities to objects by wrapping them in an additional layer of functionality.
```cpp
#include <iostream>

// Component: The interface for objects that can be decorated
class Component {
public:
    virtual void operation() = 0;
};

// Concrete Component: The base object to be decorated
class ConcreteComponent : public Component {
public:
    void operation() override {
        std::cout << "ConcreteComponent operation" << std::endl;
    }
};

// Decorator: The base class for decorators
class Decorator : public Component {
protected:
    Component* component_;

public:
    Decorator(Component* component) : component_(component) {}

    void operation() override {
        component_->operation();
    }
};

// Concrete Decorator A: Adds additional behavior to the component
class ConcreteDecoratorA : public Decorator {
public:
    ConcreteDecoratorA(Component* component) : Decorator(component) {}

    void operation() override {
        Decorator::operation();
        addBehavior();
    }

    void addBehavior() {
        std::cout << "Added behavior by ConcreteDecoratorA" << std::endl;
    }
};

// Concrete Decorator B: Adds additional behavior to the component
class ConcreteDecoratorB : public Decorator {
public:
    ConcreteDecoratorB(Component* component) : Decorator(component) {}

    void operation() override {
        Decorator::operation();
        addBehavior();
    }

    void addBehavior() {
        std::cout << "Added behavior by ConcreteDecoratorB" << std::endl;
    }
};

int main() {
    // Create a ConcreteComponent object
    Component* component = new ConcreteComponent();

    // Wrap the component with ConcreteDecoratorA
    Component* decoratorA = new ConcreteDecoratorA(component);
    decoratorA->operation();

    std::cout << std::endl;

    // Wrap the component with ConcreteDecoratorB
    Component* decoratorB = new ConcreteDecoratorB(component);
    decoratorB->operation();

    std::cout << std::endl;

    // Wrap the component with both ConcreteDecoratorA and ConcreteDecoratorB
    Component* decoratorAB = new ConcreteDecoratorA(new ConcreteDecoratorB(component));
    decoratorAB->operation();

    // Clean up
    delete decoratorAB;
    delete decoratorB;
    delete decoratorA;
    delete component;

    return 0;
}
```
In this example, the Decorator design pattern is used to add additional behavior to an object dynamically at runtime, without changing its interface.

The Component class represents the interface for objects that can be decorated. It declares a pure virtual operation() method that must be implemented by the concrete components.

The ConcreteComponent class is the base object to be decorated. It implements the Component interface and provides the base behavior.

The Decorator class is the base class for decorators. It also implements the Component interface and internally holds a pointer to a Component object. The Decorator class delegates the operation() method to the wrapped component.

The ConcreteDecoratorA and ConcreteDecoratorB classes are concrete decorators that add additional behavior to the component. They inherit from the Decorator class and override the operation() method. In their overridden method, they call the base Decorator implementation of operation() and then add their specific behavior.

In the main() function, we create a ConcreteComponent object and wrap it with different decorators (ConcreteDecoratorA and ConcreteDecoratorB). We call the operation() method on each decorator, and the added behavior is executed along with the base behavior of the component.

When you run the program, it will output:
```cpp
ConcreteComponent operation
Added behavior by ConcreteDecoratorA


ConcreteComponent operation
Added behavior by ConcreteDecoratorB


ConcreteComponent operation
Added behavior by ConcreteDecoratorB
Added behavior by ConcreteDecoratorA
```
- Composite:
Treats a group of objects as a single entity, allowing the composition of objects into tree-like structures.
```cpp
#include <iostream>
#include <vector>

// Component: The interface for all components, including composite and leaf
class Component {
public:
    virtual void operation() = 0;
};

// Leaf: Represents individual objects
class Leaf : public Component {
public:
    void operation() override {
        std::cout << "Leaf operation" << std::endl;
    }
};

// Composite: Represents a collection of components
class Composite : public Component {
private:
    std::vector<Component*> components_;

public:
    void addComponent(Component* component) {
        components_.push_back(component);
    }

    void removeComponent(Component* component) {
        // Find and remove the component
        auto it = std::find(components_.begin(), components_.end(), component);
        if (it != components_.end()) {
            components_.erase(it);
        }
    }

    void operation() override {
        std::cout << "Composite operation" << std::endl;

        // Call operation on all child components
        for (Component* component : components_) {
            component->operation();
        }
    }
};

int main() {
    // Create leaf components
    Component* leaf1 = new Leaf();
    Component* leaf2 = new Leaf();

    // Create a composite component
    Component* composite = new Composite();

    // Add leaf components to the composite
    ((Composite*)composite)->addComponent(leaf1);
    ((Composite*)composite)->addComponent(leaf2);

    // Call operation on the composite
    composite->operation();

    // Clean up
    delete composite;
    delete leaf2;
    delete leaf1;

    return 0;
}
```
In this example, the Composite design pattern is used to treat individual objects (Leaf) and collections of objects (Composite) uniformly. The composite pattern allows clients to interact with both individual objects and groups of objects in a transparent manner.

The Component class represents the interface for all components, including composite and leaf. It declares a pure virtual operation() method that must be implemented by both leaf and composite classes.

The Leaf class represents individual objects. It implements the Component interface and provides the behavior specific to leaf objects.

The Composite class represents a collection of components. It also implements the Component interface and internally holds a collection of child components (components_ vector in this case). The Composite class provides methods to add and remove child components. In the operation() method, it performs its own operation and then recursively calls the operation() method on all child components.

In the main() function, we create leaf components (Leaf) and a composite component (Composite). We add the leaf components to the composite using the addComponent() method. Then, we call the operation() method on the composite, which in turn calls the operation() method on all its child components.

When you run the program, it will output:
```cpp
Composite operation
Leaf operation
Leaf operation
```
The composite pattern allows you to treat individual objects and groups of objects in a unified way. Clients can interact with both leaf and composite objects using the same interface, allowing for hierarchical structures to be built and manipulated easily.

- Proxy:
Provides a surrogate or placeholder for another object to control access to it.
```cpp
#include <iostream>

class Subject
{
    public:
    ~Subject(){std::cout << "Releasing resource." << std::endl;}
    virtual void request() = 0;
};

class RealSubject: public Subject
{
    public:
    void request() override{
        std::cout << "RealSubject: Handling request." << std::endl;
    }
};

class Proxy: public Subject
{
    public:

    Proxy(bool condition):control(condition){}

    ~Proxy(){delete realsubjet;}

    void request() override{
        if(this->checkRequest())
            realsubjet->request();
        else
            std::cout << "Proxy: Handling request." << std::endl;
    } 

    private:

    bool checkRequest(){
        return control? true:false;
    }
    bool control;
    Subject* realsubjet = new RealSubject();
};

int main(int argc, char const *argv[])
{
    std::cout << "In case of a true condition." << std::endl;
    Subject* proxy1 = new Proxy(true);
    proxy1->request();

    std::cout << "\nIn case of a true condition." << std::endl;
    Subject* proxy2 = new Proxy(false);
    proxy2->request();

    delete proxy1;
    delete proxy2;
    return 0;
}
```
In this example, the Proxy design pattern is used to provide a surrogate or placeholder for another object (RealSubject). The Proxy controls access to the RealSubject, allowing for additional operations to be performed before or after the request is delegated to the RealSubject.

The Subject class represents the common interface for both the RealSubject and Proxy. It declares a pure virtual request() method that must be implemented by both classes.

The RealSubject class is the real object that the Proxy represents. It implements the Subject interface and provides the actual implementation of the request() method.

The Proxy class acts as a surrogate or placeholder for the RealSubject. It also implements the Subject interface and internally holds a pointer to a RealSubject object. In its request() method, the Proxy performs additional operations before and after delegating the request to the RealSubject.
When you run the program, it will output:
```cpp
In case of a true condition.
RealSubject: Handling request.

In case of a true condition.
Proxy: Handling request.
```
- Bridge:
It is used to separate out the interface from its implementation. Doing this gives the flexibility so that both can vary independently. 
```cpp
#include <iostream>
#include <string>

// Implementor: The interface for the implementation classes
class Implementor {
public:
    virtual void operationImpl() = 0;
};

// Concrete Implementor A
class ConcreteImplementorA : public Implementor {
public:
    void operationImpl() override {
        std::cout << "ConcreteImplementorA: Operation implementation A" << std::endl;
    }
};

// Concrete Implementor B
class ConcreteImplementorB : public Implementor {
public:
    void operationImpl() override {
        std::cout << "ConcreteImplementorB: Operation implementation B" << std::endl;
    }
};

// Abstraction: The interface for the abstraction classes
class Abstraction {
protected:
    Implementor* implementor_;

public:
    Abstraction(Implementor* implementor) : implementor_(implementor) {
    }

    virtual void operation() = 0;
};

// Refined Abstraction A
class RefinedAbstractionA : public Abstraction {
public:
    RefinedAbstractionA(Implementor* implementor) : Abstraction(implementor) {
    }

    void operation() override {
        std::cout << "RefinedAbstractionA: Operation implementation using ";
        implementor_->operationImpl();
    }
};

// Refined Abstraction B
class RefinedAbstractionB : public Abstraction {
public:
    RefinedAbstractionB(Implementor* implementor) : Abstraction(implementor) {
    }

    void operation() override {
        std::cout << "RefinedAbstractionB: Operation implementation using ";
        implementor_->operationImpl();
    }
};

int main() {
    ConcreteImplementorA implementorA;
    ConcreteImplementorB implementorB;

    RefinedAbstractionA abstractionA(&implementorA);
    RefinedAbstractionB abstractionB(&implementorB);

    abstractionA.operation();
    abstractionB.operation();

    return 0;
}
```
In this example, the Bridge design pattern is used to separate the abstraction (abstraction classes) from the implementation (implementor classes) and allows them to vary independently.

The Implementor class represents the interface for the implementation classes. It declares a operationImpl() method that will be implemented by the concrete implementor classes.

The ConcreteImplementorA and ConcreteImplementorB classes are concrete implementations of the implementor. They both inherit from the Implementor class and provide their own implementations of the operationImpl() method.

The Abstraction class represents the interface for the abstraction classes. It has a reference to an implementor object and declares a operation() method that will be implemented by the refined abstraction classes.

The RefinedAbstractionA and RefinedAbstractionB classes are refined abstractions that inherit from the Abstraction class. They provide their own implementations of the operation() method and use the implementor object to perform the operation.

In the main() function, we create instances of the concrete implementor classes (ConcreteImplementorA and ConcreteImplementorB) and instances of the refined abstraction classes (RefinedAbstractionA and RefinedAbstractionB), passing the appropriate implementor objects to each.

When you run the program, it will output:
```cpp
RefinedAbstractionA: Operation implementation using ConcreteImplementorA: Operation implementation A
RefinedAbstractionB: Operation implementation using ConcreteImplementorB: Operation implementation B
```
The Bridge design pattern allows the abstraction and implementation to vary independently. It provides a way to decouple the abstraction hierarchy from its implementation

#### Behavioral Patterns

- Observer:
Defines a one-to-many dependency between objects, so that when one object changes state, all its dependents are notified and updated automatically.
```cpp
#include <iostream>
#include <vector>

// Observer: The interface for all observers
class Observer {
public:
    virtual void update(int data) = 0;
};

// Subject: The interface for the subject being observed
class Subject {
private:
    int data_;
    std::vector<Observer*> observers_;

public:
    void attach(Observer* observer) {
        observers_.push_back(observer);
    }

    void detach(Observer* observer) {
        // Find and remove the observer
        auto it = std::find(observers_.begin(), observers_.end(), observer);
        if (it != observers_.end()) {
            observers_.erase(it);
        }
    }

    void notify() {
        for (Observer* observer : observers_) {
            observer->update(data_);
        }
    }

    void setData(int data) {
        data_ = data;
        notify();
    }
};

// Concrete Observer A
class ConcreteObserverA : public Observer {
public:
    void update(int data) override {
        std::cout << "ConcreteObserverA received update: " << data << std::endl;
    }
};

// Concrete Observer B
class ConcreteObserverB : public Observer {
public:
    void update(int data) override {
        std::cout << "ConcreteObserverB received update: " << data << std::endl;
    }
};

int main() {
    // Create subject and observers
    Subject subject;
    Observer* observerA = new ConcreteObserverA();
    Observer* observerB = new ConcreteObserverB();

    // Attach observers to the subject
    subject.attach(observerA);
    subject.attach(observerB);

    // Set data on the subject
    subject.setData(10);

    std::cout << std::endl;

    // Detach observerA from the subject
    subject.detach(observerA);

    // Set data on the subject again
    subject.setData(20);

    // Clean up
    delete observerB;
    delete observerA;

    return 0;
}
```
In this example, the Observer design pattern is used to establish a one-to-many dependency between objects. When the state of the subject (being observed) changes, all the registered observers are notified automatically.

The Observer class represents the interface for all observers. It declares a pure virtual update() method that must be implemented by concrete observer classes.

The Subject class represents the subject being observed. It internally holds a state (data_ in this case) and a collection of observers. The attach() method is used to register an observer, the detach() method is used to unregister an observer, and the notify() method is used to notify all registered observers when the state changes. The setData() method is used to update the state of the subject and trigger the notification of observers.

The ConcreteObserverA and ConcreteObserverB classes are concrete observers. They inherit from the Observer class and implement the update() method to define their specific behavior when receiving updates from the subject.

In the main() function, we create a Subject object and two observers (ConcreteObserverA and ConcreteObserverB). We attach both observers to the subject using the attach() method. Then, we set the data on the subject using the setData() method, which triggers the notification of observers. Each observer receives the update and performs its specific behavior.

When you run the program, it will output:
```cpp
ConcreteObserverA received update: 10
ConcreteObserverB received update: 10

ConcreteObserverB received update: 20
```
- Strategy:
Encapsulates interchangeable algorithms or behaviors and allows the algorithm to be selected at runtime.
```cpp
#include <iostream>

// Strategy: The interface for all strategies
class Strategy {
public:
    virtual void execute() = 0;
};

// Concrete Strategy A
class ConcreteStrategyA : public Strategy {
public:
    void execute() override {
        std::cout << "Executing ConcreteStrategyA" << std::endl;
    }
};

// Concrete Strategy B
class ConcreteStrategyB : public Strategy {
public:
    void execute() override {
        std::cout << "Executing ConcreteStrategyB" << std::endl;
    }
};

// Context: The context that uses a strategy
class Context {
private:
    Strategy* strategy_;

public:
    Context(Strategy* strategy) : strategy_(strategy) {}

    void setStrategy(Strategy* strategy) {
        strategy_ = strategy;
    }

    void executeStrategy() {
        strategy_->execute();
    }
};

int main() {
    // Create strategies
    Strategy* strategyA = new ConcreteStrategyA();
    Strategy* strategyB = new ConcreteStrategyB();

    // Create context with strategy A
    Context context(strategyA);
    context.executeStrategy();

    std::cout << std::endl;

    // Change the strategy of the context to strategy B
    context.setStrategy(strategyB);
    context.executeStrategy();

    // Clean up
    delete strategyB;
    delete strategyA;

    return 0;
}
```
In this example, the Strategy design pattern is used to define a family of algorithms (strategies) and make them interchangeable within a context object. The context object can switch between different strategies dynamically at runtime.

The Strategy class represents the interface for all strategies. It declares a pure virtual execute() method that must be implemented by concrete strategy classes.

The ConcreteStrategyA and ConcreteStrategyB classes are concrete strategies. They inherit from the Strategy class and provide their own implementation of the execute() method, representing different algorithms or behaviors.

The Context class represents the context that uses a strategy. It holds a reference to a strategy object and provides methods to set the strategy and execute the strategy. The executeStrategy() method delegates the execution of the strategy to the current strategy object.

In the main() function, we create two strategies (ConcreteStrategyA and ConcreteStrategyB). We then create a context object with strategyA as the initial strategy and call the executeStrategy() method, which executes the strategy A. After that, we change the strategy of the context to strategyB using the setStrategy() method and call executeStrategy() again, which now executes the strategy B.

When you run the program, it will output:
```cpp
Executing ConcreteStrategyA

Executing ConcreteStrategyB
```
The Strategy design pattern allows algorithms or behaviors to be encapsulated in separate classes and selected at runtime. This enables flexibility and interchangeability of strategies within a context object without affecting the client code.

- Template Method: 
Defines the skeleton of an algorithm in a base class but lets subclasses override specific steps of the algorithm without changing its structure.
```cpp
#include <iostream>

// Abstract Class: The template class defining the algorithm structure
class AbstractClass {
public:
    void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
    }

    virtual void primitiveOperation1() = 0;
    virtual void primitiveOperation2() = 0;
};

// Concrete Class A: Implements the primitive operations
class ConcreteClassA : public AbstractClass {
public:
    void primitiveOperation1() override {
        std::cout << "ConcreteClassA: Primitive Operation 1" << std::endl;
    }

    void primitiveOperation2() override {
        std::cout << "ConcreteClassA: Primitive Operation 2" << std::endl;
    }
};

// Concrete Class B: Implements the primitive operations
class ConcreteClassB : public AbstractClass {
public:
    void primitiveOperation1() override {
        std::cout << "ConcreteClassB: Primitive Operation 1" << std::endl;
    }

    void primitiveOperation2() override {
        std::cout << "ConcreteClassB: Primitive Operation 2" << std::endl;
    }
};

int main() {
    AbstractClass* classA = new ConcreteClassA();
    AbstractClass* classB = new ConcreteClassB();

    // Execute the template method on class A
    classA->templateMethod();

    std::cout << std::endl;

    // Execute the template method on class B
    classB->templateMethod();

    // Clean up
    delete classB;
    delete classA;

    return 0;
}
```
In this example, the Template Method design pattern is used to define the skeleton of an algorithm in an abstract class, allowing subclasses to provide specific implementations for certain steps of the algorithm.

The AbstractClass is the abstract class that defines the template method, which is the algorithm's structure. The template method calls the primitive operations, which are abstract methods that must be implemented by concrete subclasses. The primitiveOperation1() and primitiveOperation2() methods represent the steps of the algorithm that can vary across different implementations.

The ConcreteClassA and ConcreteClassB are concrete subclasses that inherit from the AbstractClass. They provide their own implementations of the primitive operations.

In the main() function, we create instances of ConcreteClassA and ConcreteClassB. We then call the templateMethod() on each object, which executes the algorithm defined in the abstract class. The primitive operations are called according to the specific implementation in each concrete class.

When you run the program, it will output:
```cpp
ConcreteClassA: Primitive Operation 1
ConcreteClassA: Primitive Operation 2

ConcreteClassB: Primitive Operation 1
ConcreteClassB: Primitive Operation 2
```
The Template Method design pattern allows for the definition of an algorithm's structure in a base class while delegating the implementation details to concrete subclasses. It provides a way to enforce the overall structure of the algorithm while allowing variations in specific steps, promoting code reuse and providing a clear separation of concerns.

- Command: 
Encapsulates a request as an object, allowing you to parameterize clients with different requests, queue or log requests, and support undoable operations.
```cpp
#include <iostream>
#include <string>

// Receiver: The target of the command
class Receiver {
public:
    void action(const std::string& message) {
        std::cout << "Receiver is performing action: " << message << std::endl;
    }
};

// Command: The interface for all commands
class Command {
public:
    virtual void execute() = 0;
};

// Concrete Command: Implements a specific command
class ConcreteCommand : public Command {
private:
    Receiver* receiver_;
    std::string message_;

public:
    ConcreteCommand(Receiver* receiver, const std::string& message)
        : receiver_(receiver), message_(message) {}

    void execute() override {
        receiver_->action(message_);
    }
};

// Invoker: Initiates and executes the commands
class Invoker {
private:
    Command* command_;

public:
    void setCommand(Command* command) {
        command_ = command;
    }

    void executeCommand() {
        command_->execute();
    }
};

int main() {
    // Create a receiver
    Receiver* receiver = new Receiver();

    // Create a command with the receiver and a message
    Command* command = new ConcreteCommand(receiver, "Command message");

    // Create an invoker and set the command
    Invoker invoker;
    invoker.setCommand(command);

    // Execute the command through the invoker
    invoker.executeCommand();

    // Clean up
    delete command;
    delete receiver;

    return 0;
}
```
In this example, the Command design pattern is used to encapsulate a request as an object, thereby allowing parameterization of clients with different requests, queue or log requests, and support undoable operations.

The Receiver class represents the target of the command. It defines the action that the command will perform.

The Command class is the interface for all commands. It declares a pure virtual execute() method that must be implemented by concrete command classes.

The ConcreteCommand class is a concrete implementation of a specific command. It takes a Receiver object and a message as parameters. The execute() method calls the action() method on the receiver, passing the message.

The Invoker class initiates and executes the commands. It holds a reference to a command object and provides methods to set the command and execute it. The executeCommand() method delegates the execution of the command to the current command object.

In the main() function, we create a Receiver object and a ConcreteCommand object with the receiver and a message. We then create an Invoker object and set the command using the setCommand() method. Finally, we call the executeCommand() method on the invoker, which executes the command by calling its execute() method.

When you run the program, it will output:
```cpp
Receiver is performing action: Command message
```
The Command design pattern decouples the object making the request (invoker) from the object receiving and performing the request (receiver). It allows for the encapsulation of requests as objects, enabling the parameterization and manipulation of requests at runtime.

- Iterator: 
Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
```cpp
#include <iostream>
#include <vector>

// Iterator: The interface for all iterators
class Iterator {
public:
    virtual bool hasNext() const = 0;
    virtual int next() = 0;
};

// Concrete Iterator: Implements the iterator interface
class ConcreteIterator : public Iterator {
private:
    std::vector<int> collection_;
    int index_;

public:
    ConcreteIterator(const std::vector<int>& collection) : collection_(collection), index_(0) {}

    bool hasNext() const override {
        return index_ < collection_.size();
    }

    int next() override {
        return collection_[index_++];
    }
};

// Aggregate: The interface for the aggregate (collection) that can be iterated
class Aggregate {
public:
    virtual Iterator* createIterator() const = 0;
};

// Concrete Aggregate: Implements the aggregate interface
class ConcreteAggregate : public Aggregate {
private:
    std::vector<int> collection_;

public:
    void addElement(int element) {
        collection_.push_back(element);
    }

    Iterator* createIterator() const override {
        return new ConcreteIterator(collection_);
    }
};

int main() {
    // Create an aggregate and add elements
    ConcreteAggregate aggregate;
    aggregate.addElement(1);
    aggregate.addElement(2);
    aggregate.addElement(3);

    // Create an iterator from the aggregate
    Iterator* iterator = aggregate.createIterator();

    // Iterate over the elements using the iterator
    while (iterator->hasNext()) {
        int element = iterator->next();
        std::cout << "Element: " << element << std::endl;
    }

    // Clean up
    delete iterator;

    return 0;
}
```
In this example, the Iterator design pattern is used to provide a way to access the elements of an aggregate (collection) sequentially without exposing its internal representation.

The Iterator class represents the interface for all iterators. It declares two methods: hasNext() to check if there are more elements, and next() to retrieve the next element.

The ConcreteIterator class is a concrete implementation of the iterator. It takes a collection as a parameter and tracks the current position (index) within the collection. The hasNext() method checks if there are more elements, and the next() method returns the next element and advances the index.

The Aggregate class represents the interface for the aggregate (collection) that can be iterated. It declares a method createIterator() to create an iterator object.

The ConcreteAggregate class is a concrete implementation of the aggregate. It holds a collection of elements and provides a method addElement() to add elements to the collection. The createIterator() method returns a ConcreteIterator object created with the collection.

In the main() function, we create a ConcreteAggregate object and add some elements to it. We then create an iterator object using the createIterator() method of the aggregate. We iterate over the elements using the iterator, calling hasNext() to check if there are more elements and next() to retrieve each element.

When you run the program, it will output:
```cpp
Element: 1
Element: 2
Element: 3
```
The Iterator design pattern provides a standardized way to access the elements of a collection sequentially, regardless of the specific implementation of the collection. It decouples the client code from the internal structure of the collection and allows for different iteration algorithms to be applied to the same collection.

- Visitor: 
Separates an algorithm from an object structure by moving the operations on the elements of the structure into separate visitor classes.
```cpp
#include <iostream>

// Forward declaration
class ConcreteElementA;
class ConcreteElementB;

// Visitor: The interface for all visitors
class Visitor {
public:
    virtual void visit(ConcreteElementA* element) = 0;
    virtual void visit(ConcreteElementB* element) = 0;
};

// Element: The interface for all elements
class Element {
public:
    virtual void accept(Visitor* visitor) = 0;
};

// Concrete Element A
class ConcreteElementA : public Element {
public:
    void accept(Visitor* visitor) override{
        visitor->visit(this);
    }

    void operationA() {
        std::cout << "Operation A of ConcreteElementA" << std::endl;
    }
};

// Concrete Element B
class ConcreteElementB : public Element {
public:
    void accept(Visitor* visitor) override{
        visitor->visit(this);
    }

    void operationB() {
        std::cout << "Operation B of ConcreteElementB" << std::endl;
    }
};

// Concrete Visitor
class ConcreteVisitor : public Visitor {
public:
    void visit(ConcreteElementA* element) override{
        std::cout << "ConcreteVisitor visits ConcreteElementA" << std::endl;
        element->operationA();
    }

    void visit(ConcreteElementB* element) override{
        std::cout << "ConcreteVisitor visits ConcreteElementB" << std::endl;
        element->operationB();
    }
};

int main() {
    ConcreteElementA elementA;
    ConcreteElementB elementB;

    ConcreteVisitor visitor;

    elementA.accept(&visitor);
    std::cout << std::endl;
    elementB.accept(&visitor);

    return 0;
}
```
In this example, the Visitor design pattern is used to separate the operations performed on elements from the structure of the elements themselves.

The Visitor class represents the interface for all visitors. It declares a visit() method for each concrete element class.

The Element class represents the interface for all elements. It declares an accept() method that takes a Visitor object as a parameter.

The ConcreteElementA and ConcreteElementB classes are concrete implementations of elements. They both inherit from the Element class and provide their own implementations of the accept() method. Additionally, they have their own specific operations (operationA() and operationB(), respectively).

The ConcreteVisitor class is a concrete implementation of a visitor. It inherits from the Visitor class and provides implementations for each visit() method, where it performs the respective operations on the elements.

In the main() function, we create instances of ConcreteElementA and ConcreteElementB, as well as a ConcreteVisitor. We then call the accept() method on each element, passing the visitor as an argument. This allows the visitor to perform the appropriate operation on each element based on its type.

When you run the program, it will output:
```cpp
ConcreteVisitor visits ConcreteElementA
Operation A of ConcreteElementA

ConcreteVisitor visits ConcreteElementB
Operation B of ConcreteElementB
```
The Visitor design pattern allows for the separation of operations performed on elements from the element structure itself. It enables new operations to be added without modifying the element classes. Visitors can visit different types of elements and perform specific operations based on the element's type. This pattern is useful when the structure of elements is stable, but new operations need to be added frequently.

- MVC (Model-View-Controller): 
Separates the application into three interconnected components: the model, the view, and the controller.
```cpp
#include <iostream>
#include <string>
#include <vector>

// Model: Represents the data and business logic
class Model {
private:
    std::vector<std::string> data_;

public:
    void addData(const std::string& item) {
        data_.push_back(item);
    }

    void removeData(const std::string& item) {
        for (auto it = data_.begin(); it != data_.end(); ++it) {
            if (*it == item) {
                data_.erase(it);
                break;
            }
        }
    }

    void displayData() const {
        std::cout << "Data: ";
        for (const auto& item : data_) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }

    std::vector<std::string> getData(){
        return data_;
    }
};

// View: Represents the presentation layer
class View {
public:
    void displayData(const std::vector<std::string>& data) const {
        std::cout << "View: Displaying data" << std::endl;
        for (const auto& item : data) {
            std::cout << item << std::endl;
        }
    }
};

// Controller: Acts as an intermediary between the model and view
class Controller {
private:
    Model model_;
    View view_;

public:
    void addData(const std::string& item) {
        model_.addData(item);
    }

    void removeData(const std::string& item) {
        model_.removeData(item);
    }

    void updateView() {
        const std::vector<std::string>& data = model_.getData();
        view_.displayData(data);
    }
};

int main() {
    Controller controller;

    // Add data
    controller.addData("Item 1");
    controller.addData("Item 2");
    controller.addData("Item 3");

    // Display data
    controller.updateView();

    // Remove data
    controller.removeData("Item 2");

    // Display data again
    controller.updateView();

    return 0;
}
```
In this example, the MVC (Model-View-Controller) design pattern is used to separate the concerns of data management (Model), user interface (View), and user input handling (Controller).

The Model class represents the data and business logic of the application. It provides methods for adding and removing data items and a method to display the current data.

The View class represents the presentation layer of the application. It provides a method to display the data.

The Controller class acts as an intermediary between the model and view. It contains instances of both the model and view classes and provides methods to manipulate the data in the model and update the view.

In the main() function, we create an instance of the Controller class. We add some data items using the addData() method and display the initial data using the updateView() method. Then, we remove a data item using the removeData() method and display the updated data again using the updateView() method.

When you run the program, it will output:
```cpp
View: Displaying data
Item 1
Item 2
Item 3
View: Displaying data
Item 1
Item 3
```
The MVC design pattern separates the concerns of data management, user interface, and user input handling, allowing for easier maintenance and reusability of code. The model represents the data and business logic, the view handles the presentation of data, and the controller acts as an intermediary, facilitating communication between the model and view. This pattern promotes a clear separation of