package org.example;
import com.vaadin.flow.component.button.Button;
import com.vaadin.flow.component.grid.Grid;
import com.vaadin.flow.component.orderedlayout.HorizontalLayout;
import com.vaadin.flow.component.orderedlayout.VerticalLayout;
import com.vaadin.flow.component.textfield.NumberField;
import com.vaadin.flow.component.textfield.TextField;
import com.vaadin.flow.server.PWA;
import com.vaadin.flow.router.Route;
import java.io.*;
import java.util.ArrayList;
@Route
@PWA(name = "My Application", shortName = "My Application")
public class MainView extends HorizontalLayout
{
    TextField name = new TextField("NAME");
    TextField sname = new TextField("SECOND NAME");
    TextField age = new TextField("AGE");
    TextField email = new TextField("EMAIL");
    Button add = new Button("ADD", event -> add());

    Button save = new Button("SAVE", event -> save());
    Button load = new Button("LOAD", event -> load());
    NumberField file = new NumberField("FILE NAME");
    Grid<Person> grid = new Grid<>(Person.class);
    VerticalLayout menu = new VerticalLayout();
    VerticalLayout grid_layout = new VerticalLayout();

    ArrayList<Person> list = new ArrayList<>();

    public MainView()
    {
        init();

        menu.add(name);
        menu.add(sname);
        menu.add(age);
        menu.add(email);
        menu.add(add);
        menu.setWidth("300px");

        grid_layout.add(save);
        grid_layout.add(load);
        grid_layout.add(file);
        grid_layout.add(grid);

        add(menu);
        add(grid_layout);
    }

    public void init()
    {
        grid.setItems(list);
        grid.setSelectionMode(Grid.SelectionMode.NONE);

    }

    public void add()
    {
        list.add(new Person(name.getValue(), sname.getValue(), email.getValue(), age.getValue()));
        grid.getDataProvider().refreshAll();
        grid.setItems(list);
    }


    public void save()
    {
        try(BufferedWriter bw = new BufferedWriter(new FileWriter("files\\" + file.getValue().intValue() + ".txt")))
        {
            list.forEach(p ->
            {
                try
                {
                    bw.write(p.getName() + "," + p.getSname() + "," + p.getEmail() + "," + p.getAge() + "\n");
                } catch (IOException e)
                {
                    e.printStackTrace();
                }
            });
        }
        catch(IOException ex)
        {

            System.out.println(ex.getMessage());
        }
    }

    public void load()
    {
        try(BufferedReader br = new BufferedReader(new FileReader("files\\" + file.getValue().intValue() + ".txt")))
        {
            String s;
            String[] s1;
            list.clear();
            while((s=br.readLine())!=null)
            {
                s1 = s.split(",");
                list.add(new Person(s1[0], s1[1], s1[2], s1[3]));
            }
            grid.getDataProvider().refreshAll();
            grid.setItems(list);
        }
        catch (FileNotFoundException e)
        {

        }
        catch (IOException e)
        {

        }
        grid.getDataProvider().refreshAll();
        grid.setItems(list);
    }
}










































package org.example;
public class Person

 {
     public String name = "";
     public String sname = "";
     public String email = "";
     public String age = "";
     public Person(String name, String sname, String email, String age)
     {
         this.name = name;
         this.sname = sname;
         this.email = email;
         this.age = age;

     }

     public String getName() {
         return name;
     }

     public void setName(String name) {
         this.name = name;
     }

     public String getSname() {
         return sname;
     }

     public void setSname(String sname) {
         this.sname = sname;
     }

     public String getEmail() {
         return email;
     }

     public void setEmail(String email) {
         this.email = email;
     }

     public String getAge() {
         return age;
     }

     public void setAge(String age) {
         this.age = age;
     }

 }
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>26jan</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>My Application</name>
    <packaging>war</packaging>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <failOnMissingWebXml>false</failOnMissingWebXml>
        <vaadin.version>14.1.5</vaadin.version>
    </properties>

    <repositories>
        <!-- The order of definitions matters. Explicitly defining central here to make sure it has the highest priority. -->

        <!-- Main Maven repository -->
        <repository>
            <id>central</id>
            <url>https://repo.maven.apache.org/maven2</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
        <!-- Repository used by many Vaadin add-ons: https://vaadin.com/directory -->
        <repository>
            <id>Vaadin Directory</id>
            <url>https://maven.vaadin.com/vaadin-addons</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.vaadin</groupId>
                <artifactId>vaadin-bom</artifactId>
                <type>pom</type>
                <scope>import</scope>
                <version>${vaadin.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>com.vaadin</groupId>
            <!-- Replace artifactId with vaadin-core to use only free components -->
            <artifactId>vaadin</artifactId>
            <exclusions>
                <!-- Webjars are only needed when running in Vaadin 13 compatibility mode -->
                <exclusion>
                    <groupId>com.vaadin.webjar</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.webjars.bowergithub.insites</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.webjars.bowergithub.polymer</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.webjars.bowergithub.polymerelements</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.webjars.bowergithub.vaadin</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.webjars.bowergithub.webcomponents</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- Added to provide logging output as Flow uses -->
        <!-- the unbound SLF4J no-operation (NOP) logger implementation -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <defaultGoal>jetty:run</defaultGoal>
        <plugins>
            <!-- We use jetty plugin, replace it with your favourite developing servlet container -->
            <plugin>
                <groupId>org.eclipse.jetty</groupId>
                <artifactId>jetty-maven-plugin</artifactId>
                <version>9.4.15.v20190215</version>
                <configuration>
                    <scanIntervalSeconds>1</scanIntervalSeconds>
                </configuration>
            </plugin>

            <!--
                Take care of synchronizing java dependencies and imports in
                package.json and main.js files.
                It also creates webpack.config.js if not exists yet.
            --><plugin>
            <groupId>com.github.eirslett</groupId>
            <artifactId>frontend-maven-plugin</artifactId>
            <!-- Use the latest released version:
            https://repo1.maven.org/maven2/com/github/eirslett/frontend-maven-plugin/ -->
            <version>1.8.0</version>

            <executions>
                <execution>
                    <!-- optional: you don't really need execution ids, but it looks nice in your build log. -->
                    <id>install node and npm</id>
                    <goals>
                        <goal>install-node-and-npm</goal>
                    </goals>
                    <!-- optional: default phase is "generate-resources" -->
                    <phase>generate-resources</phase>
                </execution>
            </executions>
            <configuration>
                <nodeVersion>v10.16.2</nodeVersion>

                <!-- optional: with node version greater than 4.0.0 will use npm provided by node distribution -->
                <!--                    <npmVersion>2.15.9</npmVersion>-->

                <!-- optional: where to download node and npm from. Defaults to https://nodejs.org/dist/ -->
                <!--                    <downloadRoot>http://myproxy.example.org/nodejs/</downloadRoot>-->
            </configuration>
        </plugin>
            <plugin>
                <groupId>com.vaadin</groupId>
                <artifactId>vaadin-maven-plugin</artifactId>
                <version>14.1.5</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>prepare-frontend</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

    <profiles>
        <profile>
            <!-- Production mode is activated using -Pproduction -->
            <id>production</id>
            <properties>
                <vaadin.productionMode>true</vaadin.productionMode>
            </properties>

            <dependencies>
                <dependency>
                    <groupId>com.vaadin</groupId>
                    <artifactId>flow-server-production-mode</artifactId>
                </dependency>
            </dependencies>

            <build>
                <plugins>
                    <plugin>
                        <groupId>com.vaadin</groupId>
                        <artifactId>vaadin-maven-plugin</artifactId>
                        <version>${vaadin.version}</version>
                        <executions>
                            <execution>
                                <goals>
                                    <goal>build-frontend</goal>
                                </goals>
                                <phase>compile</phase>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
</project>

