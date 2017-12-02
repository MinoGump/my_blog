title: 常用的设计模式
author: Mino Gump
date: 2016-08-25 23:44:40
tags:
- Design Pattern
categories:
- Tech
- Software Engineering
---

# 设计模式总结

---

## 创建型模式

### 简单工厂模式

工厂模式是很常用的一种创建型设计模式，而简单工厂模式是工厂模式中最简单的一种。在我们
需要创建不同种类的产品时，我们最好能够利用简单工厂类进行带参数的创建产品。
简单来说，由于不同的种类产品属于产品类的子类，它们有各自的子类方法，但是产品父类不知道子类的情况。
所以要创建产品时应该用一个工厂类引用所有产品类。并通过参数的方式实现创建各个产品。

![](/img/design-pattern/SimpleFactory.png)

#### 代码分析

```cpp
///////////////////////////////////////////////////////////
//  Factory.cpp
//  Implementation of the Class Factory
//  Created on:      01-十月-2014 18:41:33
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Factory.h"
#include "ConcreteProductA.h"
#include "ConcreteProductB.h"
Product* Factory::createProduct(string proname){
	if ( "A" == proname )
	{
		return new ConcreteProductA();
	}
	else if("B" == proname)
	{
		return new ConcreteProductB();
	}
	return  NULL;
}
```

#### 模式分析

- 将对象的创建和对象本身业务处理分离可以降低系统的耦合度，使得两者修改起来都相对容易。
- 在调用工厂类的工厂方法时，由于工厂方法是静态方法，使用起来很方便，可通过类名直接调用，而且只需要传入一个简单的参数即可，在实际开发中，还可以在调用时将所传入的参数保存在XML等格式的配置文件中，修改参数时无须修改任何源代码。
- 简单工厂模式最大的问题在于工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑，这一点与开闭原则是相违背的。
- 简单工厂模式的要点在于：当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节。

#### 模式优点

- 工厂类的创建函数含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例，客户端可以直接使用产品（相当于自适应）
- 简单工厂模式通过这种做法实现了对责任的分割，它提供了专门的工厂类用于创建对象。
- 通过引入配置文件，可以在不修改任何客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性。

#### 模式缺点

- 由于工厂类集中了所有产品创建逻辑，一旦不能正常工作，整个系统都要受到影响。
- 使用简单工厂模式将会增加系统中类的个数，在一定程序上增加了系统的复杂度和理解难度。
- 系统扩展困难，一旦添加新产品就不得不修改工厂逻辑，在产品类型较多时，有可能造成工厂逻辑过于复杂，不利于系统的扩展和维护。
- 简单工厂模式由于使用了静态工厂方法，造成工厂角色无法形成基于继承的等级结构。


---

### 工厂方法模式

工厂方法模式其实是简单工厂模式的拓展，当存在多个简单工厂时，我们可以用一个工厂总类，让所有的简单工厂类继承它。
简单来说，工厂方法模式是通过重载创建产品的方法，实现代码重用。

![](/img/design-pattern/FactoryMethod.png)

#### 代码分析

```cpp
///////////////////////////////////////////////////////////
//  ConcreteFactory.cpp
//  Implementation of the Class ConcreteFactory
//  Created on:      02-十月-2014 10:18:58
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteFactory.h"
#include "ConcreteProduct.h"

Product* ConcreteFactory::factoryMethod(){

	return  new ConcreteProduct();
}
```

```cpp
#include "Factory.h"
#include "ConcreteFactory.h"
#include "Product.h"
#include <iostream>
using namespace std;

int main(int argc, char *argv[])
{
	Factory * fc = new ConcreteFactory();
	Product * prod = fc->factoryMethod();
	prod->use();
	
	delete fc;
	delete prod;
	
	return 0;
}
```

#### 模式分析

工厂方法模式由于利用了面向对象的多态性，重用了代码的同时，更解决了简单工厂模式系统扩展难的问题。
在工厂方法模式中，核心的工厂类不再负责所有产品的创建，而是将具体创建工作交给子类去做。
这个核心类仅仅负责给出具体工厂必须实现的接口，而不负责哪一个产品类被实例化这种细节，这使得工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品。


#### 模式优点

- 在工厂方法模式中，工厂方法用来创建客户所需要的产品，同时还向客户隐藏了哪种具体产品类将被实例化这一细节，用户只需要关心所需产品对应的工厂，无须关心创建细节，甚至无须知道具体产品类的类名。
- 基于工厂角色和产品角色的多态性设计是工厂方法模式的关键。它能够使工厂可以自主确定创建何种产品对象，而如何创建这个对象的细节则完全封装在具体工厂内部。工厂方法模式之所以又被称为多态工厂模式，是因为所有的具体工厂类都具有同一抽象父类。
- 使用工厂方法模式的另一个优点是在系统中加入新产品时，无须修改抽象工厂和抽象产品提供的接口，无须修改客户端，也无须修改其他的具体工厂和具体产品，而只要添加一个具体工厂和具体产品就可以了。这样，系统的可扩展性也就变得非常好，完全符合“开闭原则”。

#### 模式缺点

- 在添加新产品时，需要编写新的具体产品类，而且还要提供与之对应的具体工厂类，系统中类的个数将成对增加，在一定程度上增加了系统的复杂度，有更多的类需要编译和运行，会给系统带来一些额外的开销。
- 由于考虑到系统的可扩展性，需要引入抽象层，在客户端代码中均使用抽象层进行定义，增加了系统的抽象性和理解难度，且在实现时可能需要用到DOM、反射等技术，增加了系统的实现难度。



---

### 抽象工厂模式

抽象工厂模式其实挺好用的，当我们要构建多个产品，多种产品的情况下，这种方式应该是最佳选择。一个抽象工厂类管理所有子工厂，每个子工厂负责创建新的子产品。下面我们来看看它的类图

![](/img/design-pattern/AbstractFactory.png)

#### 代码分析

```cpp
///////////////////////////////////////////////////////////
//  ConcreteFactory1.cpp
//  Implementation of the Class ConcreteFactory1
//  Created on:      02-十月-2014 15:04:11
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteFactory1.h"
#include "ProductA1.h"
#include "ProductB1.h"
AbstractProductA * ConcreteFactory1::createProductA(){
	return new ProductA1();
}


AbstractProductB * ConcreteFactory1::createProductB(){
	return new ProductB1();
}
```

```cpp
///////////////////////////////////////////////////////////
//  ProductA1.cpp
//  Implementation of the Class ProductA1
//  Created on:      02-十月-2014 15:04:17
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ProductA1.h"
#include <iostream>
using namespace std;
void ProductA1::use(){
	cout << "use Product A1" << endl;
}
```

```cpp
#include <iostream>
#include "AbstractFactory.h"
#include "AbstractProductA.h"
#include "AbstractProductB.h"
#include "ConcreteFactory1.h"
#include "ConcreteFactory2.h"
using namespace std;

int main(int argc, char *argv[])
{
	AbstractFactory * fc = new ConcreteFactory1();
	AbstractProductA * pa =  fc->createProductA();
	AbstractProductB * pb = fc->createProductB();
	pa->use();
	pb->eat();
	
	AbstractFactory * fc2 = new ConcreteFactory2();
	AbstractProductA * pa2 =  fc2->createProductA();
	AbstractProductB * pb2 = fc2->createProductB();
	pa2->use();
	pb2->eat();
```

#### 模式分析

抽象工厂模式综合了工厂模式生产产品的便捷性，同时也提供了优良的产品管理，以至于不会出现不同的产品用同一个工厂生产的矛盾现象。
看来，抽象工厂模式就是现实生活中的真实工厂啊！比如衣服厂生产衣服和裤子、玩具厂生产各式各样的玩具。

#### 模式优点

- 抽象工厂模式隔离了具体类的生成，使得客户并不需要知道什么被创建。由于这种隔离，更换一个具体工厂就变得相对容易。所有的具体工厂都实现了抽象工厂中定义的那些公共接口，因此只需改变具体工厂的实例，就可以在某种程度上改变整个软件系统的行为。另外，应用抽象工厂模式可以实现高内聚低耦合的设计目的，因此抽象工厂模式得到了广泛的应用。
- 当一个产品族中的多个对象被设计成一起工作时，它能够保证客户端始终只使用同一个产品族中的对象。这对一些需要根据当前环境来决定其行为的软件系统来说，是一种非常实用的设计模式。
- 增加新的具体工厂和产品族很方便，无须修改已有系统，符合“开闭原则”。

#### 模式缺点

- 在添加新的产品对象时，难以扩展抽象工厂来生产新种类的产品，这是因为在抽象工厂角色中规定了所有可能被创建的产品集合，要支持新种类的产品就意味着要对该接口进行扩展，而这将涉及到对抽象工厂角色及其所有子类的修改，显然会带来较大的不便。
- 开闭原则的倾斜性（增加新的工厂和产品族容易，增加新的产品等级结构麻烦）。

---

### 建造者模式

建造者模式是将复杂对象整理到一起的模式，我通常称其为导演模式，其实就是cocos的模式。Director负责创建Builder，而Builder负责创建具体的Product。
在Cocos中，也是有一个单例的Director，director负责管理所有的场景，包括场景的切换、初始化等等。而每个的Scene负责创建该场景的事件以及角色。
当然了，这个Director也可以制造多个Builder，Builder也可以制造多个Product。
说起来，建造者模式和抽象工厂模式还有点像呢！但是呢，抽象工厂模式是通过面向对象语言中的继承来实现的，每个子工厂其实是继承了父工厂（抽象工厂），因此父工厂不能获取子工厂的具体信息。
而建造者模式是通过调用对象的方式，单例director调用各个Builder，这样director相当于是站在了上帝视角管理所有生产工作。

![](/img/design-pattern/Builder.png)

#### 代码分析

```cpp
#include <iostream>
#include "ConcreteBuilder.h"
#include "Director.h"
#include "Builder.h"
#include "Product.h"

using namespace std;

int main(int argc, char *argv[])
{
	ConcreteBuilder * builder = new ConcreteBuilder();
	Director  director;
	director.setBuilder(builder);
	Product * pd =  director.constuct();
	pd->show();
	
	delete builder;
	delete pd;
	return 0;
}
```

```cpp
///////////////////////////////////////////////////////////
//  ConcreteBuilder.cpp
//  Implementation of the Class ConcreteBuilder
//  Created on:      02-十月-2014 15:57:03
//  Original author: colin
///////////////////////////////////////////////////////////

#include "ConcreteBuilder.h"


ConcreteBuilder::ConcreteBuilder(){

}



ConcreteBuilder::~ConcreteBuilder(){

}

void ConcreteBuilder::buildPartA(){
	m_prod->setA("A Style "); //不同的建造者，可以实现不同产品的建造  
}


void ConcreteBuilder::buildPartB(){
	m_prod->setB("B Style ");
}


void ConcreteBuilder::buildPartC(){
	m_prod->setC("C style ");
}
```

```cpp
///////////////////////////////////////////////////////////
//  Director.cpp
//  Implementation of the Class Director
//  Created on:      02-十月-2014 15:57:01
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Director.h"

Director::Director(){
}

Director::~Director(){
}

Product* Director::constuct(){
	m_pbuilder->buildPartA();
	m_pbuilder->buildPartB();
	m_pbuilder->buildPartC();
	
	return m_pbuilder->getResult();
}


void Director::setBuilder(Builder* buider){
	m_pbuilder = buider;
}
```

#### 模式分析

建造者模式的特定是引入了Director类，这个类不仅隔离了生产与客户，而且它还控制着整个生产的流程。在客户端中，用户只需要知道director的实例，即可调用director的方法制造产品。
这对用户而言真是一个好消息啊！

#### 模式优点

- 在建造者模式中， 客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象。
- 每一个具体建造者都相对独立，而与其他的具体建造者无关，因此可以很方便地替换具体建造者或增加新的具体建造者， 用户使用不同的具体建造者即可得到不同的产品对象 。
- 可以更加精细地控制产品的创建过程 。将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰，也更方便使用程序来控制创建过程。
- 增加新的具体建造者无须修改原有类库的代码，指挥者类针对抽象建造者类编程，系统扩展方便，符合“开闭原则”。

#### 模式缺点

- 建造者模式所创建的产品一般具有较多的共同点，其组成部分相似，如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制。
- 如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大。


---

### 单例模式

单例模式是最常见的模式之一了，单例模式的宗旨就是唯一呀，这个实例只能有一个。比如说任务管理器只能有一个。那么如何实现呢？全局变量可以控制实例数量，但是这不安全。
所以单例模式是通过私有化构造函数和实例来实现的，这样外部不能调用构造函数，只能调用一个getInstance的函数，getInstance公共函数负责判断当前单例是否为空，为空则构造新的单例，否则返回这个单例，保证了实例唯一。

![](/img/design-pattern/Singleton.png)

#### 代码分析

```cpp
#include <iostream>
#include "Singleton.h"
using namespace std;

int main(int argc, char *argv[])
{
	Singleton * sg = Singleton::getInstance();
	sg->singletonOperation();
	
	return 0;
}
```

```cpp
///////////////////////////////////////////////////////////
//  Singleton.cpp
//  Implementation of the Class Singleton
//  Created on:      02-十月-2014 17:24:46
//  Original author: colin
///////////////////////////////////////////////////////////

#include "Singleton.h"
#include <iostream>
using namespace std;

Singleton * Singleton::instance = NULL;
Singleton::Singleton(){

}

Singleton::~Singleton(){
	delete instance;
}

Singleton* Singleton::getInstance(){
	if (instance == NULL)
	{
		instance = new Singleton();
	}
	
	return  instance;
}


void Singleton::singletonOperation(){
	cout << "singletonOperation" << endl;
}
```

#### 模式分析

单例模式其实是最简单的模式了吧，单例类不需要和其他类进行通信，所以只需要构造自身的getInstance函数即可。实现方法就不多说了。
单例模式能够实现单个实例，这对于管理类来说是必须的条件，因为管理者只需要一个。同时单例模式能够防止多个实例被同时实例化。

#### 模式优点

- 提供了对唯一实例的受控访问。因为单例类封装了它的唯一实例，所以它可以严格控制客户怎样以及何时访问它，并为设计及开发团队提供了共享的概念。
- 由于在系统内存中只存在一个对象，因此可以节约系统资源，对于一些需要频繁创建和销毁的对象，单例模式无疑可以提高系统的性能。
- 允许可变数目的实例。我们可以基于单例模式进行扩展，使用与单例控制相似的方法来获得指定个数的对象实例。

#### 模式缺点

- 由于单例模式中没有抽象层，因此单例类的扩展有很大的困难。
- 单例类的职责过重，在一定程度上违背了“单一职责原则”。因为单例类既充当了工厂角色，提供了工厂方法，同时又充当了产品角色，包含一些业务方法，将产品的创建和产品的本身的功能融合到一起。
- 滥用单例将带来一些负面问题，如为了节省资源将数据库连接池对象设计为单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出；现在很多面向对象语言(如Java、C#)的运行环境都提供了自动垃圾回收的技术，因此，如果实例化的对象长时间不被利用，系统会认为它是垃圾，会自动销毁并回收资源，下次利用时又将重新实例化，这将导致对象状态的丢失。
