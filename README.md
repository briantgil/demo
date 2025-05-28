# How to Create an Executable .jar file for JavaFX using Maven

0. Requirements
    - Set JAVA_HOME environment variable and add JDK to PATH
    - Add Maven to PATH
    - VS Code Extension Pack for Java

1. Create Java Project > JavaFX (create from archetype): Provided by Maven for Java

2. Create a New Main.java Class

```java
package com.example;

public class Main {
    public static void main(String[] args) {
        App.main(args);
    }
}
```

3. App.java

```java
package com.example;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.stage.Stage;
import javafx.scene.image.Image;

import java.io.IOException;

/**
 * JavaFX App
 */
public class App extends Application {

    private static Scene scene;

    @Override
    public void start(Stage stage) throws IOException {
        scene = new Scene(loadFXML("primary"), 640, 480);
        //scene.getStylesheets().add(getClass().getResource("styles.css").toExternalForm());

        Image icon = new Image("IMG_4730.jpg");  //put image in src\main\java

        stage.getIcons().add(icon);
        stage.setTitle("My App");
        stage.setResizable(false);  

        stage.setScene(scene);
        stage.show();
    }

    static void setRoot(String fxml) throws IOException {
        scene.setRoot(loadFXML(fxml));
    }

    private static Parent loadFXML(String fxml) throws IOException {
        FXMLLoader fxmlLoader = new FXMLLoader(App.class.getResource(fxml + ".fxml"));
        return fxmlLoader.load();
    }

    public static void main(String[] args) {
        launch();
    }

}
```

4. Update Your Maven pom.xml File

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>3.5.0</version>
    <executions>
    <execution>
        <phase>package</phase>
        <goals>
        <goal>shade</goal>
        </goals>
        <configuration>
        <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
            <mainClass>com.example.Main</mainClass>
            </transformer>
        </transformers>
        </configuration>
    </execution>
    </executions>
</plugin>
```

5. Building and Testing the .jar File

```powershell
mvn clean
#test
mvn javafx:run -f pom.xml
mvn install
```

6. Locating the .jar Files
    - After running mvn install, you’ll find two .jar files in the target directory of your project
    - The .jar file that does not start with "original" is the executable .jar file that includes all your project’s dependencies
    - Navigate to the target directory in your project folder and double-click the .jar file
  
```powersehll
java -jar path-to-jar\demo-1.0-SNAPSHOT.jar 
```

