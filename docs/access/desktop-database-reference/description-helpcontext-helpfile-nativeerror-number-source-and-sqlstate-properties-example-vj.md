---
<<<<<<< Titre tête : Description, HelpContext, HelpFile, propriétés-exemple (VJ ++) TOCTitle : Description, HelpContext, HelpFile, NativeError, nombre, Source et SQLState, propriétés-exemple (VJ ++) === titre : Description, HelpContext, HelpFile, propriétés-exemple (VJ ++) TOCTitle : Description, HelpContext, HelpFile, NativeError, nombre, Source et SQLState, propriétés-exemple (VJ ++)
>>>>>>> Master ms:assetid : daa3ff89-9f7f-f832-479e-bbb51c918ae8 ms:mtpsurl : https://msdn.microsoft.com/library/JJ250100(v=office.15) ms:contentKeyID : ms.date 48548085 : 18/09/2015 mtps_version : v=office.15
---

<<<<<<< Tête
# <a name="description-helpcontext-helpfile-nativeerror-number-source-and-sqlstate-properties-example-vj"></a>Description, HelpContext, HelpFile, NativeError, Number, Source et SQLState, propriétés - Exemple (VJ++)
=======
# <a name="description-helpcontext-helpfile-nativeerror-number-source-and-sqlstate-properties-example-vj"></a>Description, HelpContext, HelpFile, NativeError, nombre, Source et SQLState, propriétés-exemple (VJ ++)
>>>>>>> master


**S’applique à**: Access 2013 | Office 2013

Cet exemple déclenche une erreur, l'intercepte et affiche les propriétés [Description](description-property-ado.md), [HelpContext](helpcontext-helpfile-properties-ado.md), [HelpFile](helpcontext-helpfile-properties-ado.md), [NativeError](nativeerror-property-ado.md), [Number](number-property-ado.md), [Source](source-property-ado-error.md) et [SQLState](sqlstate-property-ado.md) de l'objet [Error](error-object-ado.md) qui en résulte.

```java
    // BeginDescriptionJ
    // The WFC class includes the ADO objects.
    
    import com.ms.wfc.data.*;
    import java.io.* ;
    
    public class DescriptionX
    {
       // The main entry point for the application.
    
       public static void main (String[] args)
       {
          DescriptionX();
          System.exit(0);
       }
    
       // DescriptionX function
    
       static void DescriptionX()
       {
          // Declarations.
          BufferedReader in = new 
             BufferedReader(new InputStreamReader(System.in));
    
          // Define ADO Objects.
          Connection cnConn1 = null;
    
          try
          {
             // Intentionally trigger an error.
             cnConn1 = new Connection();
             cnConn1.open("nothing");
          }
          catch( AdoException ae )
          {
             // Notify user of any errors that result from ADO.
             PrintProviderError(cnConn1);
          }
    
          try
          {
             System.out.println("\nPress <Enter> key to continue.");
             in.readLine();
          }
          // System read requires this catch.
          catch( java.io.IOException je)
          {
             PrintIOError(je);
          }
      
       }
    
       // PrintProviderError Function
    
       static void PrintProviderError( Connection Cnn1 )
       {
          // Print Provider errors from Connection object.
          // ErrItem is an item object in the Connections Errors collection.
          com.ms.wfc.data.Error  ErrItem = null;
          long nCount = 0;
          int  i      = 0;
    
          nCount = Cnn1.getErrors().getCount();
    
          // If there are any errors in the collection, print them.
          if( nCount > 0);
          {
             // Collection ranges from 0 to nCount - 1
             for (i = 0; i< nCount; i++)
             {
                ErrItem = Cnn1.getErrors().getItem(i);
                System.out.println("\t Error number: " + ErrItem.getNumber()
                   + "\t" + ErrItem.getDescription() );
             }
          }
    
       }
    
       // PrintIOError Function
    
       static void PrintIOError( java.io.IOException je)
       {
          System.out.println("Error \n");
          System.out.println("\tSource = " + je.getClass() + "\n");
          System.out.println("\tDescription = " + je.getMessage() + "\n");
       }
    }
    // EndDescriptionJ
```
