# PMDM 1ª EV: Android

## Actividades e Intents: 
# Intents: 
> Un **Intent** es un objeto que proporciona **vinculación** en tiempo de ejecución entre ciertos componentes separados, como **dos actividades**.
>



**Existen 2 tipos de Intents:**

 - `Explícitos`: Inician una actividad determinada.
 - `Implícitos`: Inician un tipo de actividad. 

Podemos iniciar un **Intent** vacío o pasándole datos con los **métodos** `putExtra`() y `setData`() e iniciarlo con el **método** `startActivity`().



## Intents Explícitos:

Dentro de los Intents Explícitos se nos pueden dar distintas posibilidades según las necesidades:

### **Cuando no necesitemos pasarle datos para iniciar la actividad**

>    ##### `Intents explícitos básicos` : 
>
>    Aquí creamos el objeto Intent vacío y lo iniciamos con `startActivity`(nombreIntent).
>
>    MainActivity:
>
>    ```Java
>    Intent intent = new Intent(MainActivity.this,MessageActivity.class); 
>    startActivity(intent); //iniciar intent
>    ```
>     MessageActivity:
>
>    ```java
>    Intent intent = getIntent();
>    ```
>
>    

### **Cuando necesitemos pasarle datos para iniciar la actividad**

Aquí existen dos posibilidades; los **EXTRA** y los **DATA**, con sus **métodos** `putExtra`() y `setData`() .

#### DATA

Los **DATA** solo pueden contener objetos **URI** ( http://, tel://, geo:// ), se utilizan cuando solo hay que pasar una información (que pueda representarse en un URI).

>    ##### Intents explícitos con `setData`()  : 
>
>    Aquí creamos el objeto Intent y le pasamos una URL HTTP con `setData`().
>
>    **MainActivity:**
>
>    ```Java
>    Intent withData = new Intent(MainActivity.this,MessageActivity.class); 
>    withData.setData(Uri.parse("http://www.google.com"));
>    startActivity(withData); //iniciar intent
>    ```
>    **MessageActivity**
>
>    ```java
>    Intent intent = getIntent();
>    Uri sampleUri = intent.getData();
>    ```
>

#### EXTRA

Los **EXTRA** permiten pasar cualquier información, se almacena en objetos **key-value** (Bundle) y se usa cuando necesitamos transmitir mas de una información y cuando esta no puede representarse con un URI. 																						  Se pueden pasar tantos datos como sean necesarios.

Hay dos formas de declarar los EXTRA:

>    ##### Intents explícitos con `putExtra`(): 
>
>    > Aquí creamos el objeto Intent y le pasamos un **String** y un **Integer** con el método `putExtra`() de forma directa sin crear el objeto Bundle.
>    >
>
>    **MainActivity:**
>
>    ```Java
>    Intent wExtra = new Intent(MainActivity.this,MessageActivity.class); 
>    wExtra.putExtra("EXTRA_STR","este es mi String");
>    wExtra.putExtra("EXTRA_INT",999);
>    startActivity(wExtra); //iniciar intent
>    ```
>    **MessageActivity**:
>
>    ```java
>    Intent intent = getIntent();
>    String extraStr = intent.getStringExtra("EXTRA_STR");
>    int extraInt = intent.getIntExtra("EXTRA_INT",1) //1 = defaultValue
>    ```
>

>    ##### Recibiendo respuesta de una sub-actividad con `startActivityForResult`(): 
>
> >   Aquí creamos el objeto Intent y el objeto Bundle que pasaremos como parámetro al método >>`putExtras`() del Intent.
>
>**MainActivity:**
>    
>    ```Java
>    Intent wExtra = new Intent(MainActivity.this,MessageActivity.class); 
>wExtra.putExtra("EXTRA_STR","este es mi String");
>    startActivityForResult(wExtra, REQUEST_CODE); //iniciar intent
>    ```
>    **MessageActivity:**
>    
>    ```java
>    Intent intent = getIntent();
>    String extraStr = intent.getStringExtra("EXTRA_STR");
>    Intent returnIntent = new Intent();
>    returnIntent.putExtra("EXTRA_RETURN_STR","klk");
>setResult(RESULT_OK,returnIntent);
>    finish();
>    ```
>    

>    ##### Intents explícitos con `putExtras`(Bundle extras): 
>
>    Aquí creamos el objeto Intent y el objeto Bundle que pasaremos como parámetro al método `putExtras`() del Intent.
>
>    Para popular objeto Bundle lo haremos con los métodos `putString`(), `putInt`(), etc.
>    	
>    **MainActivity:**
>
>    ```Java
>    Intent wExtra = new Intent(MainActivity.this,MessageActivity.class); 
>    Bundle extras = new Bundle();
>    extras.putString("EXTRA_STR","este es mi String");
>    extras.putInt("EXTRA_INT",999);
>    wExtra.putExtras(extras);
>    startActivity(wExtra); //iniciar intent
>    ```
>    **MessageActivity:**
>
>    ```java
>    Intent intent = getIntent();
>    String extraStr = intent.getStringExtra("EXTRA_STR");
>    int extraInt = intent.getIntExtra("EXTRA_INT",1) //1 = defaultValue
>    ```
>
---------------------------------------------------------------------------------------------------------------------------

## Intents implícitos

Un Intent implícito es una instancia de la clase Intent que además de los campos Data y Extra de un Intent explícito puede contener:

- Action: Indica la acción a realizar por la actividad receptora.
- **Category** (Opcional): Proporciona información adicional sobre la categoría del componente que debe recibir la Intent
- **Type** (Opcional): Indica el tipo de datos MIME con los que debe operar la Actividad

>  ####  Intent implícito con `setAction`() sin selector de App:
>
>  ```Java
>  Intent sendIntent = new Intent(); // Declarar intent
>  sendIntent.setAction(Intent.ACTION_SEND);
>  sendIntent.putExtra(Intent.EXTRA_TEXT, "hello");
>  sendIntent.setType("text/plain");
>  if(sendIntent.resolveActivity(getPackageManager()) != null){
>      startActivity(sendIntent); //iniciamos intent
>  }
>  ```
>

>  ####  Intent implícito con `setAction`() con selector de App:
>
>  Aquí crearemos un selector de aplicación con `createChooser`(intent, "str");
>
>  ```Java
>  Intent sendIntent = new Intent(); // Declarar intent
>  Intent chooser = Intent.createChooser(sendIntent, "Choose app");
>  sendIntent.setAction(Intent.ACTION_SEND);
>  sendIntent.putExtra(Intent.EXTRA_TEXT, "hello");
>  sendIntent.setType("text/plain");
>  if(sendIntent.resolveActivity(getPackageManager()) != null){
>    startActivity(chooser); //iniciamos intent
>  }
>  ```
>

Las acciones mas comunes son: 

>  ####  Redactar un correo electrónico:
>
>  Aquí implementaremos un método para enviar correos electrónicos.
>
>  ```Java
>  public void sendMail(){
>      String[] addresses = {"dam02.2020.jesuitas@gmail.com","another"};
>      String message = "mensaje - body";
>      String subject = "asunto";
>      
>      Intent sendIntent = new Intent();
>      
>      sendIntent.setAction(Intent.ACTION_SENDTO);
>      sendIntent.setData(Uri.parse("mailto:"));
>      sendIntent.putExtra(Intent.EXTRA_TEXT, message);
>      sendIntent.putExtra(Intent.EXTRA_EMAIL, addresses);		   	 		sendIntent.putExtra(Intent.EXTRA_SUBJECT, subject);
>      
>      if(sendIntent.resolveActivity(getPackageManager()) != null){
>  		startActivity(sendIntent); //iniciamos intent
>      } else {
>          Toast.makeText(this, "There is no Email Client 			  Installed.",Toast.LENGTH_SHORT).show();
>      }
>  }
>  ```
>

---------------------------------------------------------------------------------------------------------------------------

# Botones :play_or_pause_button:

> Estos atributos y métodos son aplicables a los elementos **Button**, **ImageButton**, <u>**FloatingActionButton**</u> y a cualquier **View**, como por ejemplo, una **ImageView**.



#### `android:onClick`="método"

>> Aquí llamamos al método desde el Layout con el atributo `android:onClick`="método".
>>
>
> MainActivity.xml:
>
>
>```xml
><Button
>android:layout_width="wrap_content" 
>android:layout_height="wrap_content"     
>android:layout_marginTop="16dp"
>android:text="Order"
>android:onclick="nombreMetodo"/>
>```
>

MainActivity.java:

```java
public void nombreMetodo(View view){
//funcionalidad increiblemente complicada
}
```



> #### `onClickListener`()
>
> Aquí llamamos al botón desde la Clase con el método `onClickListener`() en el método `onCreate`().
>
> MainActivity.xml:
>
> ```xml
> <Button
> android:id="@+id/send_button"
> android:layout_width="wrap_content" 
> android:layout_height="wrap_content"     
> android:layout_marginTop="16dp"
> android:text="Order"/>
> ```
>
> MainActivity.java:
>
> ```java
> Button boton = findViewById(R.id.send_button);
> boton.setOnClickListener(new View.OnClickListener(){
>  public void onClick(View v){
>      TextView summaryTextView = findViewById(R.id.summary);
>      summaryTextView.setText("sape");//funcionalidad
>  }
>    });
>    ```


> #### `Floating Action Button`
>
> >Extiende de la clase **ImageButton**.
>>
> >Se usa `onClickListener`() en el método `onCreate`().
>
>
> MainActivity.xml:
>
> ```xml
> <android.support.design.widget.FloatingActionButton
>         android:id="@+id/fab"
>         android:layout_width="wrap_content"
>         android:layout_height="wrap_content"
>         android:layout_gravity="bottom|end"
>         android:layout_margin="@dimen/fab_margin"
>         android:src="@drawable/ic_fab_chat_button_white" />
> ```
>
> MainActivity.java:
>
> ```java
> FloatingActionButton fab = findViewById(R.id.fab);       
> fab.setOnClickListener(new View.OnClickListener() {
> 	public void onClick(View view) {
> 		Intent intent = new Intent(MainActivity.this,OrderActivity.class);
>         	//funcionalidad increiblemente complicada
>             }
>         });
> ```
> 

---------------------------------------------------------------------------------------------------------------------------

# Input Controls

#### `Text Input`

> >Extiende de la clase **ImageButton**.
> >
> >Se usa `onClickListener`() en el método `onCreate`().
>
> MainActivity.xml:
>
> ```xml
> <android.support.design.widget.FloatingActionButton
>         android:id="@+id/fab"
>         android:layout_width="wrap_content"
>         android:layout_height="wrap_content"
>         android:layout_gravity="bottom|end"
>         android:layout_margin="@dimen/fab_margin"
>         android:src="@drawable/ic_fab_chat_button_white" />
> ```
>
> MainActivity.java:
>
> ```java
> FloatingActionButton fab = findViewById(R.id.fab);       
> fab.setOnClickListener(new View.OnClickListener() {
> 	public void onClick(View view) {
> 		Intent intent = new Intent(MainActivity.this,OrderActivity.class);
>         	//funcionalidad increiblemente complicada
>             }
>         });
> ```
> 

#### `CheckBoxes`

> >Extiende de la clase **ImageButton**.
> >
> >Se usa `onClickListener`() en el método `onCreate`().
>
> MainActivity.xml:
>
> ```xml
> <android.support.design.widget.FloatingActionButton
>         android:id="@+id/fab"
>         android:layout_width="wrap_content"
>         android:layout_height="wrap_content"
>         android:layout_gravity="bottom|end"
>         android:layout_margin="@dimen/fab_margin"
>         android:src="@drawable/ic_fab_chat_button_white" />
> ```
>
> MainActivity.java:
>
> ```java
> FloatingActionButton fab = findViewById(R.id.fab);       
> fab.setOnClickListener(new View.OnClickListener() {
> 	public void onClick(View view) {
> 		Intent intent = new Intent(MainActivity.this,OrderActivity.class);
>         	//funcionalidad increiblemente complicada
>             }
>         });
> ```
> 
#### `Radio Buttons`

> >Extiende de la clase **ImageButton**.
> >
> >Se usa `onClickListener`() en el método `onCreate`().
>
> MainActivity.xml:
>
> ```xml
> <android.support.design.widget.FloatingActionButton
>         android:id="@+id/fab"
>         android:layout_width="wrap_content"
>         android:layout_height="wrap_content"
>         android:layout_gravity="bottom|end"
>         android:layout_margin="@dimen/fab_margin"
>         android:src="@drawable/ic_fab_chat_button_white" />
> ```
>
> MainActivity.java:
>
> ```java
> FloatingActionButton fab = findViewById(R.id.fab);       
> fab.setOnClickListener(new View.OnClickListener() {
> 	public void onClick(View view) {
> 		Intent intent = new Intent(MainActivity.this,OrderActivity.class);
>         	//funcionalidad increiblemente complicada
>             }
>         });
> ```
>
> 
 #### `Spinner`

> >#### Spinner
> >
> >Proporciona una forma rápida de seleccionar un valor de entre una lista, de, normalmente mas de tres elementos, ya que se muestra en un menú desplegable
> >
> >1. Crear elemento Spinner en Layout.
> >
> >2. Crear array de valores en el Layout.
> >
> >3. Rellenar el Spinner con los valores en el método `onCreate`().
> >
> >    a) Crea Adapter que tome los valores del Array y cree una View por cada elemento del Array con el método `createFromResource`().
> >
> >    b) Indica al Adapter en que layout queremos que añada los elementos con el método			       `setDropDownViewResource`().
> >
> >4. Asociar el Adapter al Spinner en el método `onCreate`().   
> >
> >5. Indicar acción cuando se pulse en un elemento desplegado, implementando la interfaz `OnItemSelectedListener` y asociando el método del Listener en el `onCreate`().
> >
> >6. Implementar método `onItemSelected`() con el código que queremos ejecutar cuando se pulse un elemento, podemos usar el método para acceder al elemento seleccionado a partir de su posición del array.
>
> MainActivity.xml:
>
> ```xml
> <Spinner 
>     android:id="@+id/labelSpinner"
>     android:layout_width="wrap_content"
>     android:layout_height="wrap_content"
>     android:layout_marginStart="16dp"/>
> ```
>
> strings.xml:
>
> ```xml
> <string-array name="labels_array">
>     <item>Home</item>
>     <item>Work</item>
>     <item>Mobile</item>
>     <item>Other</item>
> </string-array>
> ```
> MainActivity.java(crear spinner): 
>
> ```java
> ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(MainActivity.this, R.array.labels_array, android.R.layout.simple_spinner_item); //paso 3a
> adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);//3b
> 
> 
> Spinner spinner = findViewById(R.id.label_spinner);//4
> if(spinner != null){
>     spinner.setAdapter(adapter);
> }
> ```
>
> MainActivity.java(onClick): 
>
> ```java
> public class MainActivity extends AppCompatActivity implements AdapterView.OnItemSelectedListener {
>     
>     Spinner spinner = findViewById(R.id.label_spinner);
>     if(spinner != null){
>     spinner.setOnItemSelectedListener(this);
> 	}
> }
> ```
>
>
> ```java
> public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
>     String spinner_item = adapterView.getItemAtPosition(i).toString();
>     displayToast(spinner_item);
> }
> ```
> 
#### `Toggle Button y Switches`

> >Extiende de la clase **ImageButton**.
> >
> >Se usa `onClickListener`() en el método `onCreate`().
>
> MainActivity.xml:
>
> ```xml
> <android.support.design.widget.FloatingActionButton
>         android:id="@+id/fab"
>         android:layout_width="wrap_content"
>         android:layout_height="wrap_content"
>         android:layout_gravity="bottom|end"
>         android:layout_margin="@dimen/fab_margin"
>         android:src="@drawable/ic_fab_chat_button_white" />
> ```
>
> MainActivity.java:
>
> ```java
> FloatingActionButton fab = findViewById(R.id.fab);       
> fab.setOnClickListener(new View.OnClickListener() {
> 	public void onClick(View view) {
> 		Intent intent = new Intent(MainActivity.this,OrderActivity.class);
>         	//funcionalidad increiblemente complicada
>             }
>         });
> ```
> 
