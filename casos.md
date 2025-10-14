Casos de ejemplo para el repositorio: 

________________________
## Caso #1
________________________

Dev_1 quiere agregar un cambio al siguiente código base:

```java
public class Main {
public static void main(String[] args) {
System.out.println("Hola Mundo!");
    }
}
```

El cambio consiste en darle la bienvenida al usuario: 
```bash
git checkout dev
git pull origin qa
```

realiza el cambio 
```java

public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo!");
        System.out.println("Bienvenido a CalibrAite.");
    }
}
```

Hace commit y push 

```
git add .
git commit -m "Agrega saludo de bienvenida"
git push origin dev

```

Se crea PR de dev -> qa

QA revisa, aprueba y hacde merge. 
se promueve a staging-uat  y luego a master. 
__________________________________
 ## CASO # 2
__________________________________

Dev_1 y Dev_2 Modifican el mismo archivo 

Dev_1 Agrega un Saludo. 

```java

public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo!");
        System.out.println("Bienvenido a CalibrAite.");
    }
}
```


Dev_2 Agrega una otro mensaje.
```java

public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo!");
        System.out.println("Gran dia para usar CalibrAite.");
    }
}
```
cuando dev_2 intenta hacer push 
```
git push origin dev
```
git responde: 
```
! [rejected] dev -> dev (non-fast-forward)
```
debe hacer:
```
git pull origin dev
```
git muestra conflicto: 
```
CONFLICT (content): Merge conflict in src/Main.java
```
y el archivo queda así 
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo!");
<<<<<<< HEAD
        System.out.println("Bienvenido a CalibrAite.");
=======
        System.out.println("Gran dia para usar CalibrAite.!");
>>>>>>> origin/dev
    }
}
```
dev_2 debe fusionar ambos mensajes:
```java 
public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo!");
        System.out.println("Bienvenido a CalibrAite.");
        System.out.println("Gran dia para usar CalibrAite.");
    }
}

```

luego:

```
git add .
git commit -m "Resuelve conflicto y combina mensajes"
git push origin dev

```
__________________________________
## CASO # 3
__________________________________
QA rechaza un cambio

QA encuentra que el mensaje de fecha no debe ir.
El cambio ya fue mergeado en qa, así que se revierte.

```
git checkout qa
git pull
git log --oneline  
git revert <hash_del_commit>
git push origin qa
```
Esto crea un nuevo commit de reversión.

Luego se hace merge a staging-uat 
y finalmente si es aprobado se pasa a producción (master) sin afectar demás cambios

__________________________________
## CASO # 4
__________________________________
# Deploy final 
el flujo completo fue:

dev → qa → staging-uat → master 

en master el código final es: 


```java 
public class Main {
    public static void main(String[] args) {
        System.out.println("Hola Mundo!");
        System.out.println("Bienvenido a CalibrAite.");
    }
}

```

