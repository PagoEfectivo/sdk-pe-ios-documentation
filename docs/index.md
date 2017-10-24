## **PagoEfectivo SDK para iOS**
----

[Image here]

### **Overview**
----

_PagoEfectivoSDK_ le permitirá implementar de forma sencilla y rápida realizar transacciones online a través de la plataforma de _PagoEfectivo_ en dispositivos iPhone.

En el siguiente manual encontrará todo lo necesario para poder empezar la implementación del SDK, así como la forma correcta de implementar sus principales funciones tales como *Generar un Cip*, *Listar Cips*, etc.

### **Pre requisitos**
----
* [Xcode 8 u 9](https://developer.apple.com/xcode/)
* iOS 10+

### **Instalación**
----
Es recomendable usar _CocoaPods_ para instalar el SDK:

#### **Vía cocoapods**
Es necesario tener instalado el gestor de paquetería, para ello debes ir a  [la página web de cocoapods](https://cocoapods.org/) 
y así continuar con los pasos abajo.

**Paso 1:**
Vía consola ubícarse a la carpeta del proyecto, donde se encuentra el archivo de extensión **.xcodeproj**

**Paso 2:**
Una vez ubicado en la carpeta del proyecto, ejecutar el siguiente comando:

```bash
$ pod init
``` 

**Paso 3:**
Agrega la siguiente línea de código al archivo **Podfile**:
``` 
#!ruby
pod "PagoEfectivoSDK"
```

**Paso 5:**
Instala el pod y abre el archivo de extensión **.xworkspace**:
```bash
$ pod install
$ open yourProject.xworkspace
```

#### **Vía manual**
**Paso 1:**
Se debe descargar el [archivo **.framework**](https://goo.gl/)

**Paso 2:**
Abrir tu proyecto con Xcode, y agregar el archivo a las secciones **Embedded Binaries** en el target principal de tu proyecto

Aqui un ejemplo de cómo debe agregarse de forma manual:
![ejemplo forma manual](../assets/forma-manual.gif)

### **Inicializar Pago Efectivo SDK**
----
Para poder hacer uso de las funciones que ofrece el SDK, es necesario importar e inicializar la librería. 
Para ello debes tener los valores antes de empezar:

* Service Id
* Access Key
* Secret Key

!!! info
    Si aún no tienes los key's correspondientes favor de comunicarte [aquí](mailto:elcomercio.mobile@gmail.com)

#### En Objective-C
Para inicializar y hacer uso de los métodos del SDK de PagoEfectivo bajo un proyecto en **Objective-C**, debes seguir los siguientes pasos:

**Paso 1:**
Importa el módulo de PagoEfectivoSDK en el archivo **AppDelegate.m** de la aplicación:

```
#!obj-c
#import <PagoEfectivoSDK/PagoEfectivoSDK.h>
```

**Paso 2:**
Luego de haberlo importado, se debe agregar el siguiente código en el método **application** que se encuentra en el archivo **AppDelegate.m**:

```
#!obj-c
[PagoEfectivoSDK config: @"MY_SECRET_KEY"
                  accessKey: @"MY_ACCESS_KEY"
                  serviceId: MY_SERVICE_ID ];
```

#### En Swift 3.x ó 4.x

Crear el archivo **_MyProject_-Brigding-Header.h**, y establecer la ruta del archivo creado en la directiva **Objective-C Bridging Header** que se encuentra en la sección **Build Settings** en el _target_ principal del proyecto.

**Paso 1:**
En el archivo **_MyProject_-Brigding-Header.h** agregar lo siguiente:

```
#!obj-c
#import <PagoEfectivoSDK/PagoEfectivoSDK.h>
#import <PagoEfectivoSDK/Cip.h>
```

**Paso 2:**
En el archivo **AppDelegate.swift** agregar lo siguiente:
```
#!swift
import PagoEfectivoSDK
```

**Paso 3:**
Luego de haberlo importado, se debe agregar el siguiente código en el método **application** que se encuentra en el archivo **AppDelegate.swift**:

```
#!swift
PagoEfectivoSDK.config("MY_SECRET_KEY", accessKey: "MY_ACCESS_KEY", serviceId: MY_SERVICE_ID)
```

## **Uso de Pago Efectivo SDK | CIP's functions**
----

### **Generar un Cip**
----

En Objective-C:
```
#!obj-c
//Se crea una nueva instancia de tipo CipRequest
CipRequest *instanceRequest = [CipRequest alloc];

//Se establecen las propiedades  requeridas
[instanceRequest setAmount:100.05];
[instanceRequest setCurrency: USD];
[instanceRequest setTransactionCode:@"101"];
[instanceRequest setUserEmail:@"demo@demo.com"];

// Creamos una variable que almacene nuestra función que irá de callback para el servicio
serviceCallback __block callbackResponse = ^(long status, id receivedData, NSError *error){
        //código para tratar con la respuesta
};

// Usamos la función de generación de Cip del SDK
[[PagoEfectivoSDK Cip] generate:EN 
                  requestObject:instanceRequest 
                responseHandler:callbackResponse];
```

### **Consultar Cip(s)**
----

En Objective-C:
```
#!obj-c
//Definimos un arreglo
NSArray *cips = @[@"2495383",@"50",@"10"];

//Usamos la función de búsqueda de Cips del SDK
[[PagoEfectivoSDK Cip] search:cips responseHandler:^(long status, NSMutableArray *cipSearchArray, NSError *error) {
    if(error){
        NSLog(@">Got response with error %@.\n", error);
    }
}];
```