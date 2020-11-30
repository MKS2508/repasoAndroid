# PMDM 1ª EV: Android

## Actividades e Intents: 

### Intents:

Existen 2 tipos de Intents:

 - Explícitos: Inician una actividad determinada.

 - Implícitos: Inician un tipo de actividad. 

   #### Intents explícitos: 

```java
Intent intent = new Intent(MainActivity.this,Activity.class); // Declarar intent
startActivity(intent); //iniciamos intent
```

​	
