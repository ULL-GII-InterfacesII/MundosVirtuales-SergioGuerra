# Mundos virtuales. Introducción a la programación de gráficos 3D  
## Sergio Guerra Arencibia - ULL - Interfaces Inteligentes  
### 1.- Qué funciones se pueden usar en los scripts de Unity para llevar a cabo traslaciones, rotaciones y escalados.  
  
  Las funciones de traslación, rotación y escalado se aplican sobre la componente transform de un gameObject, así que primero debemos obtenerlo:  
  ```c#
        Transform tf = GetComponent<Transform>()
  ```  
  Posteriormente podemos usar los siguientes métodos:  
    - tf.translate(Vector3)
    - tf.rotate(Vector3)
  Para el escalado, lo mejor es acceder a la propiedad tf.localScale. A esta le añadimos un vector3 para realizar el escalado:  
    - tf.localScale = new Vector3(...)  
      
### 2.- ¿Cómo duplicarías el tamaño de un objeto en un script?  

  Primero obtenemos su componente transform:  
  ```c#
        Transform tf = GetComponent<Transform>()
  ```  
  Y posteriormente multiplicamos su componente localScale por dos:  
  ```c#
        Vector3 myVector = new Vector3(2.0f, 2.0f, 2.0f)
        tf.localScale *= myVector
  ```  
### 3.- ¿Cómo situarías un objeto en la posición (3, 5, 1)  

  La forma más sencilla es modificar su componente tranform. Esto solo se debe hacer si el objeto no obedece al motor de físicas (es decir, no tiene una comopnente rigidBody o, si la tiene, es kinematic). Si no se cumpliera esto, habría que aplciarle una fuerza para lograr ponerlo en esta posición (bastante complejo si queremos una posición exacta). Como no se especifica, asumiré que es el primer escenario  
  ```c#  
    Transform tf = GetComponent<Transform>();
    Vector3 myVector = new Vector3(3.0f, 5.0f, 1.0f)
    tf.position = myVector  

  ```  

### 4.- ¿Cómo trasladarías 3m en cada uno de los jees y luego lo rotas 30º alrededor del eje Y?  

```c#  
  Transform tf = GetComponent<Transform>();
  Vector3 translationVector = new Vector3(3.0f, 3.0f, 3.0f);
  tf.translate(translationVector);
  tf.rotate(0.0f, 30.0f, 0.0f);
```  

### 5.- ¿Cómo rotarías un objeto sobre el eje (1,1,1)?  
```c# 
  Transform tf = GetComponent<Transform>();
  Vector3 rotateAxis = new Vector3(1.0f, 1.0f, 1.0f);
  float angle = NUMBER;
  tf.rotate(rotateAxos, angle);
```  
  

### 6.- Rota un objeto alrededor del eje Y 30º y desplázalo 3 metros en cada uno de los ejes. ¿Obtendrías el mismo resultado que en 4?  

Depende. Si efectuamos las transformaciones directamente sobre los ejes de coordenadas de la escena (con el ratón), no se obtiene el mismo resultado, ya que estamos trabajando con los sistemas de referencia locales a cada objeto. Al rotar un objeto, su sist. de referencia local también rota y por tanto el resultado cambia.  

Si se realiza de manera numérica (con un script o modificando las propiedades del objeto a la derecha de la interfaz), estos cambios se realizan sobre el sistema de referencia absoluto de la escena y por tanto el resultado es el mismo sea cual sea el orden
   

### 7.- Como puedes obtener la distancia al plano cerca del volumen de vista desde la cámara  

Para obtener la distancia desde la cámara al plano cercano se utiliza el atributo *nearClipPlane* de la clase
Camera.  
  

### 8.- Como puedes aumentar el ángulo de la cámara  

El ángulo de la cámara se modifica con el atributo fieldOfView de la clase Camera.  
Cabe destacar que esta es el ángulo vertical, el horizontal depende del ratio de aspecto de la ventana (viewport's aspect ratio).
Entonces, modificando esta propiedad podemos obtener un mayor o menor ángulo.  
```c#
    // Modificamos el ángulo de la cámara principal
    Camera m_MainCamera = Camera.main;
    m_MainCamera.fieldOfView = 120.0f;  
```  

### 9.- Qué efecto tiene disminuir el ángulo de la cámara.  

Disminuir el ángulo de la camara hace que el área que se encuentra entre el plano cercano y el 
lejano disminuya, haciendo que el volumen entre estos (el volumen del llamado *frustum*) disminuya.  
Esto hace que las zonas donde la cámara capta información sean menores, habiendo menos objetos y teniendo que escalar
estos a un mayor tamaño para mostrar lo que se capta de la escena en la pantalla.  
  
### 10.- Configura un volumen de vista que recorte objetos de la escena.  

Para configurar un volumen de vista tenemos dos opciones, modificar el ángulo de la cámara (como vimos en la pregunta 8) o
modificar la distancia entre la cámara, el plano cercano y el plano lejano. Esto es debido a que son los parámetros que modifican
el volumen del *frustum*.  
Si queremos que el volumen de vista recorte objetos de la escena pues depende de dónde estén los objetos de la escena.  
A grandes rasgos, para recortar un objeto lejano deberíamos acercar el plano lejano. Si el objeto es muy cercano deberíamos 
alejar el plano cercano. Modificar el ángulo de la cámara recortará objetos que estén localizados en la periferia de la escena
que capta la cámara.  

### 11.- Como puedes realizar la proyección al espacio 2D
  
Para que una cámara realize una proyección 2D, debemos modificar la propiedad *orthographic*.  
Este atributo tiene dos valores, verdadero o falso.  
Si lo tenemos a verdadero la cámara realiza una proyección ortográfica, es decir, 2D. Si es falso la cámara
realiza una proyección en persepectiva, 3D.  

### 12.- Especifica la rotación de los apartados 4 y 6 con la utilidad quaternion.  

Rotar 30º alrededor del eje Y  
```c#  
  Transform tf = GetComponent<Transform>();
  tf.rotation = Quaternion.AngleAxis(30, Vector3.up);  
```  

### 13.- ¿Como puedes averiguar la matriz de proyección en perspectiva que se ha usado para proyectar la escena al último frame renderizado?  

Usando la propiedad de la cámara llamada *previousViewProjectionMatrix*. Esta nos devuelve la matriz de proyección usada para proyectar el último frame, 
si la cámara está con la propiedad orthographic a false, nos devolverá la matriz usada para la proyección en perspectiva.  
  
### 14.- ¿Como puedes averiguar la matriz de proyección en perspectiva ortográfica que se ha usado para proyectar la escena al último frame renderizado?

Nuevamente, usando la propiedad de la cámara llamada *previousViewProjectionMatrix*.
En este caso, la cámara debe estar con la propiedad orthographic a true, nos devolverá la matriz usada para la proyección ortográfica.  

### 15.- ¿Cómo puedes obtener la matriz de transformación entre el sistema de coordenadas local y el mundial?

Para obtener la matriz de transformación entre un sist. coordenadas local y el mundial podemos usar la propiedad *localToWorldMatrix* del
objeto transform.  
Si lo que se desea es obtener la matriz de transformación entre el espacio local de la cámara al mundial, entonces podemos usar *cameraToWorldMatrix* 
del objeto Camera.  
  
### 16.- Cómo puedes obtener la matriz para cambiar al sistema de referencia de vista  

### 17.- Especifica la matriz de la proyección usado en un instante de la ejecución del ejercicio 1 de la práctica 1
El código sería el siguiente  
```c#  
  Camera m_MainCamera = Camera.main;  
  Debug.Log(m_MainCamera.previousViewProjectionMatrix);
```

| | | | |
| ------------- |:-------------:| -----:| -----:|
| 0.75307  | 0.00000  | 0.00000  | 0.00000 |
| 0.00000  | -1.70325 | 0.314567 | 7.80836 |
| 0.00000  | -0.0005  | -0.00003 | 0.29716 |
| 0.00000  | 0.18161  | 0.98337  | 9.76363 |
  
### 18.- Especifica la matriz de modelo y vista de la escena del ejercicio 1 de la práctica 1.  

### 19.- Aplica una rotación en el start de uno de los objetos de la escena y muestra la matriz de cambio al sistema de referencias mundial  
  
El código del método start de un objeto sería el siguiente:  
```c# 
  Transform tf = GetComponent<Transform>();
  tf.rotation = Quaternion.AngleAxis(50, Vector3.up);
  Debug.Log(tf.localToWorldMatrix);  

```  
| | | | |
| ------------- |:-------------:| -----:| -----:|
| 0.64279  | 0.00000  | 0.76604  | 0.00000 |
| 0.00000  | 1.00000  | 0.00000  | 0.00000 |
| -0.76604 | 0.00000  | 0.64279  | 0.00000 |
| 0.00000  | 0.00000  | 0.00000  | 1.00000 |  
   


### 20.- ¿Como puedes calcular las coordenadas del sistema de referencia de un objeto con las siguientes propiedades del Transform? Position (3, 1, 1), Rotation (45, 0, 45)
