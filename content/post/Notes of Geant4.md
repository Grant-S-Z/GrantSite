+++
title = "Notes of Geant4"
author = ["Grant"]
lastmod = 2024-03-08T02:11:28+08:00
tags = ["Geant4"]
categories = ["physics", "cs"]
draft = false
+++

---


## Toolkit Fundamentals {#toolkit-fundamentals}

{{< figure src="./img/20240308010121-notes_of_geant4userguide.org_20240308_011216.png" >}}


### Global Usage Classes {#global-usage-classes}


#### Signature {#signature}

为了使用统一的命名方式，遵循 Geant4 的一些约定，Geant4 内核的每一个类的名字都以前缀 G4 开头。为了便于移植，在 Geant4 中使用 G4 类型(G4int, G4float, G4double 等)代替 C 中原有的类型(int, float, double 等)。

基本类型：G4int, G4long, G4float, G4double, G4bool, G4complex, G4String.

多数基本类型定义包含了头文件 globals.hh.


### System of Units {#system-of-units}


#### Input {#input}

必须对引入数据指定单位：

```c++
G4double Size = 15 * km;
G4double density = 11 * mg/cm3;
```

对于交互式命令，要求：

```mac
/gun/energy 15.2 keV
/gun/position 3 2 -7 meter
```


#### Output {#output}

可以使用喜欢的单位输出数据，只要将这些数据除以相应的单位：

```c++
G4cout << KineticEnergy/keV << " keV";
```

也可以让 Geant4 来选择使用最合适的单位来输出数据，只要指定用户数据的类别(属于 Length, Time, Energy 等)，例如：

```c++
G4cout << G4BestUnit(SetpSize, "Length");
```


### Run {#run}


#### Basic Concept {#basic-concept}

在 Geant4 中，Run 是一个最大的模拟单位，一个 run 由一系列的事件组成，它是 G4Run 的 一个对象，由 G4RunManager 的方法 beamOn() 启动。在一个 run 过程中，探测器几何，灵敏 探测器设置，和模拟中所使用的物理过程，都保持不变。

**G4RunManager** 管理一个 run 的相关进程。其公用方法：

-   initialize()

    Geant4 内核必须的所有初始化由该方法触发，包括：

    -   探测器几何
    -   粒子和物理过程
    -   反应截面等计算

    该方法必须在首个 run 执行之前调用。

-   beamOn(G4int numberOfEvent)

    触发一个实际的模拟过程。

**G4UserRunAction** 是一个用户行为类。其虚拟方法：

-   beginOfRunAction()

    在 beamOn() 开始时被调用，常用情形：

    -   设置一个 run 标识数
    -   登记一个统计图
    -   设置探测器运行条件

-   endOfRunAction()

    在 beamOn() 结束时被调用，常用情形：

    -   存储/输出统计图
    -   操作 run 摘要


### Event {#event}

G4Event 代表一个事件，它是一个包含所有被模拟事件的输入输出信息的类的实例。这个对 象在 G4RunManager 中被创建并传递给 G4EventManager。可以通过 G4RunManager 的方法 getCurrentEvent() 获取当前正在处理的事件指针。


## Detector Definition and Response {#detector-definition-and-response}


### Geometry {#geometry}


#### Solid Volume {#solid-volume}

**Box**:

```c++
G4Box(const G4String& pName, G4double pX, G4double pY, G4double pZ);
```

**Tube**:

```c++
G4Tubs(const G4String& pName, G4double pRMin, G4double pRMax, G4double pDz, G4double pSPhi, G4double pDPhi);
```

**Full Solid Sphere**:

```c++
G4Orb(const G4String& pName, G4double pRmax);
```


#### Logical Volume {#logical-volume}

逻辑体管理那些与给定实体和材料的探测器单元有关的信息，与它们在探测器中的物理位置无关。


#### Physical Volume {#physical-volume}

物理体指明了那些描述探测器单元的 volumes 的空间位置。


### Material {#material}


### Electromagnetic Field {#electromagnetic-field}


### Hits {#hits}


## Tracking and Physics {#tracking-and-physics}


### Tracking {#tracking}


### Physics Processes {#physics-processes}

物理过程描述粒子是怎样和物质发生相互作用的。

每个物理过程都有两组方法，GetPhysicalInteractionLength (GPIL) 和 DoIt，它们在粒子跟踪中扮演重要角色。GPIL 方法给出从当前时空位置到下个时空位置的步长，它根据物理过程的反应截面信息来计算发生反应的概率。在 step 的结尾调用 DoIt 方法。DoIt 方法实现了这个反应的细节，改变粒子的能量、动量、方向和位置，还产生次级粒子（如需要）。


### Particles {#particles}

G4ParticleDefinition 集中了反应一个粒子属性的信息，如名称，质量，自旋，寿命和衰变模式。


### Production Threshold versus Tracking Cut {#production-threshold-versus-tracking-cut}


### Cuts per Region {#cuts-per-region}


## User Actions {#user-actions}


### Mandatory User Actions and Initializations {#mandatory-user-actions-and-initializations}

Geant4 有三个虚类，用户为了实现模拟必须重载它们的方法。它们要求用户定义探测器，指定将要使用的物理过程，并且描述如何初始化将要产生的粒子。

-   G4VUserDetectorConstruction
-   G4VUserPhysicsList
-   G4VUserPrimaryGeneratorAction


### Optional User Actions {#optional-user-actions}

有 5 个虚类，用于增强在不同阶段对模拟的控制，用户必须重载这些类的方法。每个 action 类的每个方法都有一个空的缺省实现，允许用户继承并实现期望的类和方法。用户 action 类的对象，必须向 G4RunManager 注册。

-   G4UserRunAction
-   G4UserEventAction
-   G4UserStackingAction
-   G4UserTrackingAction
-   G4UserSteppingAction


## Control {#control}
