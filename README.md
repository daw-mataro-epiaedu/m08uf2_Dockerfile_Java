# Contenidor aplicació consola Java

## Autobuild

Exemple de com dockeritzar una aplicació senzilla de Java per consola

El programa Java és el clàssic que se li passa un enter com argument i pinta un triangle amb * amb el número de files corresponent a l'argument.

```java
import java.io.*;
public class Main 
{

    // Function to print n files of stars
    public static void printStars(int n) 
    { 
        int i, j;
  
        // loop to handle number of rows 
        for(i=0; i<n; i++)
        {
  
            //  loop to handle number of columns
            //
            for(j=0; j<=i; j++)
            { 
                // printing stars 
                System.out.print("* ");
            } 
  
            // end line
            System.out.println(); 
        }
   }
 
    public static void main(String[] args) 
    {
        try
        {
            int n = Integer.parseInt(args[0].trim());
            printStars(n);
        }
        catch(NumberFormatException error)
        {
            System.out.println("Argument must be an integer");
        }
    }
}
```

Descripció del Dockerfile

```Dockerfile
  FROM openjdk:latest
  COPY . /usr/src/myapp
  WORKDIR /usr/src/myapp
  RUN javac Main.java
  ENTRYPOINT [ "java","Main"]
  CMD ["5"]
```

> FROM: Usem com imatge base la darrera de openjdk.
>
> COPY copiem dins el contenidor a la carpeta on toca tota la carpeta de treball.
>
> WORKDIR establim com directori de treball per la resta de comandes la carpeta /usr/src/myapp.
>
> RUN aquí compilem el nostre arxiu java
>
> ENTRYPOINT serveix per executar java i li passem com argument programa compilat
>
> CMD establim un argument per defecte, per si no li passem cap, agafa el valor 5

Per provar-ho:

```bash
docker build -t demo-java .
docker run --rm demo-java
```

Amb això obtenim el següent resultat:

> \*
>
> \* \*
>
> \* \* \*
>
> \* \* \* \*
>
> \* \* \* \* \*

També li podem passar un argument específic:

```
docker run --rm demo-java 8
```

> \*
>
>\* \*
>
> \* \* \*
>
> \* \* \* \*
>
> \* \* \* \* \*
>
> \* \* \* \* \* \*
>
> \* \* \* \* \* \* \*
>
> \* \* \* \* \* \* \* \*
