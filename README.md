Download Link: https://assignmentchef.com/product/solved-cs5200-integrating-spring-boot-with-mysql
<br>
<span style="font-size: 2.61792em; letter-spacing: -1px; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu, Cantarell, 'Helvetica Neue', sans-serif;">Introduction</span>

For this course we will be using MySQL as the database for permanent data storage. We will use Java Persistence API (JPA) as an Object Relation Map (ORM) to map Java classes to SQL tables and Java object instances to records. These instructions are also documented in the following <u>​</u><a href="https://goo.gl/QFmEsk">online</a> <a href="https://goo.gl/QFmEsk">slides</a> (​<a href="https://goo.gl/QFmEsk">https://goo.gl</a> <a href="https://goo.gl/QFmEsk">/QFmEsk</a>)​ and ​<a href="https://goo.gl/i6X4ss">available</a> <a href="https://goo.gl/i6X4ss">online</a> (​<a href="https://goo.gl/i6X4ss">https://goo.gl/i6X4ss</a><u>​</u><a href="https://goo.gl/i6X4ss">)</a>. This assignment assumes the successful completion of an <u>​</u><a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">earlier assignment</a>​.

<h2>Learning Objectives</h2>

By the end of this assignment you should be able to

<ul>

 <li>Create and configure a remote MySQL database on AWS</li>

 <li>Connect to a remote MySQL database with MySQL Workbench</li>

 <li>Integrate a Spring Boot Web project with a remote MySQL database</li>

 <li>Map Java POJOs to a relational schema using Java Persistence API</li>

 <li>Expose a data model through a Web service endpoint</li>

</ul>

<h1>Creating a Remote MySQL Instance on Heroku</h1>

This section describes creating a remote MySQL database instance running on Heroku. These steps assume you have successfully deployed a Spring Boot application on Heroku. If not, make sure to follow the instructions for setting up a remote account, remote application, and a development environment as described in <u>​</u><a href="https://docs.google.com/presentation/d/1jkBylv9qA-ULEJmlx9Asb378dOdn2xj2tU5kDBksXJU/edit?usp=sharing">Settingup</a> <a href="https://docs.google.com/presentation/d/1jkBylv9qA-ULEJmlx9Asb378dOdn2xj2tU5kDBksXJU/edit?usp=sharing">a</a> <a href="https://docs.google.com/presentation/d/1jkBylv9qA-ULEJmlx9Asb378dOdn2xj2tU5kDBksXJU/edit?usp=sharing">Development</a> <a href="https://docs.google.com/presentation/d/1jkBylv9qA-ULEJmlx9Asb378dOdn2xj2tU5kDBksXJU/edit?usp=sharing">Environment</a> lecture. Once the environment is setup, create and deploy a Spring Boot application on Heroku as describe in <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">Deploying</a> <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">Spring</a> <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">Boot</a> <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">Web</a> <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">Applications</a> <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">to</a> <a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">Heroku</a><a href="https://docs.google.com/presentation/d/1Dt3CqNEFOBuJYa5MvslzINEYphKWjkR6fydm_b62X_0/edit?usp=sharing">.</a><u>​</u> Finally, add a remote MySQL database to the remote development environment as described in ​<a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Adding</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">a</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Remote</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">MySQL</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Database</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">to</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">a</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Spring</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Boot</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Web</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Application</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">on</a> <a href="https://docs.google.com/presentation/d/19Z0dEz-_6vgt0S492NjEqfP8nfTKtYjpb8UOfyWfLZ4/edit?usp=sharing">Heroku</a>​. The general steps for setting up the environment on Heroku are listed below. Refer to the original documents for more details.

<ol>

 <li>Install <u>​</u><a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html">JDK 8</a>​ or later</li>

 <li>Install <u>​</u><a href="https://maven.apache.org/download.cgi">Apache Maven</a></li>

 <li>Install the ​<a href="https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started-installing-spring-boot.html#getting-started-installing-the-cli">Spring Boot</a>​ framework</li>

 <li>Install <a href="https://dev.mysql.com/downloads/workbench/">MySQL Workbenc</a><u>​ </u><a href="https://dev.mysql.com/downloads/workbench/">h</a>​ or some other MySQL client</li>

 <li>Create an account on <u>​</u><a href="https://signup.heroku.com/">Heroku</a></li>

 <li>Install the ​<a href="https://devcenter.heroku.com/articles/heroku-cli">Heroku CLI</a></li>

 <li>Create a simple Spring Boot Web application, e.g., spring init –dependencies=web myapp</li>

</ol>




<ol start="8">

 <li>Deploy the Spring Boot application to Heroku, e.g., heroku create</li>

</ol>




<ol start="9">

 <li>Add a MySQL remote database to the remote Spring Boot application on Heroku</li>

 <li>Connect your local MySQL Workbench to the remote MySQL on Heroku</li>

</ol>




The connection string will contain the username, password, host and port. Make a note of this since you’ll need it in later steps of this guide.

<h1>Creating a Remote MySQL Instance on AWS</h1>

This section describes creating a remote MySQL database instance running on AWS.




<ol>

 <li>Login to the Amazon AWS console and expand <em>All Services</em>​ ​.</li>

 <li>Under the ​<em>Database</em>​ section, select ​<em>RDS</em>​.</li>

 <li>In the ​<em>Amazon RDS</em> landing page, select ​<em>Launch</em> or ​<em>Get Started Now</em> to add a new RDS instance.</li>

 <li>In the ​<em>Select engine</em>​ screen, select ​<em>MySQL</em>​ and then click ​<em>Next</em>​.</li>

 <li>In the ​<em>Choose use case</em>​ screen, select ​<em>Dev/Test – MySQL</em>​ and then click <em>Next</em>​ ​.</li>

 <li>In the ​<em>Specify DB details</em> screen, keep the default settings, and choose the following configuration and then click ​<em>Next</em>​. Use your own identifier, username, and password. The usernames, names, and identifiers shown in this document are based on a particular course, e.g., ​cs5200​, semeter, e.g., ​fall2018​, and your lastname, e.g., annunziato. Please use your particular values where applicable.​</li>

</ol>




<ol start="2">

 <li>DB instance class: ​t2.micro</li>

 <li>DB instance identifier: ​cs5200-fall2018-annunziato</li>

 <li>Master username: ​jannunzi</li>

 <li>Master password: yourPa$$word123​</li>

 <li>Confirm password: ​yourPa$$word123</li>

</ol>




Make note of the actual values used above since they will be used in later steps.




<ol start="7">

 <li>In the ​<em>Configure advanced settings</em> screen, select <em>Yes</em>​ for the <em>Public</em>​<em> accessibility</em>​. Also, keep the default settings, but choose the name of the database, e.g., ​cs5200_ fall2018_annunziato​. Note the use of underscores. This will be the name of the schema where tables and their records will be stored. Click on <em>Launch</em>​<em> DB Instance</em> to continue. The database will take a few moments to be created after which you can navigate to it by clicking on ​<em>View DB Instance Details</em>​ or clicking ​<em>Instances</em>​ on the left.</li>

</ol>




<ol start="8">

 <li>The details screen will be titled with the DB instance identifier chosen earlier, e.g., cs5200-fall2018-annunziato​. Scroll down to the ​<em>Connect</em> Make a note of</li>

</ol>

the ​<em>Endpoint</em> since it will be used later to connect remotely to the database. The endpoint is the name of the server machine where the database is running, e.g.,




cs5200-fall2018-annunziato.cne500ro4imj.us-west-2.rds.amazonaws.com




Also note the ​<em>Port</em> where the server is listening for incoming connections, e.g., ​3306.​ If the ​<em>Endpoint</em> is not yet available, wait a few more minutes while the database service is setup.




<ol start="9">

 <li>Note that the connection might not be publicly available by default as denoted by the configuration ​<em>Publicly accessible: No​</em>. To make the connection publicly available, under the <em>Security group​</em>, click on the ​<em>Inbound</em> security group. In the security group screen, click the <em>Inbound</em> tab, and then the ​<em>Edit</em> Under the ​<em>Source</em> column, select ​<em>Anywhere</em> from the dropdown, and click ​<em>Save​</em>. Verify that the ​<em>Publicly accessible​</em> setting is set to ​<em>Yes​</em>.</li>

</ol>

<h1>Install, Configure, and Start MySQL Workbench</h1>

<em>MySQL Workbench</em> (Workbench) is a desktop application that provides a graphical user interface to easily interact with a MySQL database instance. Workbench will be used throughout to interact with MySQL running remotely on AWS or Heroku. To install Workbench, navigate to




<a href="https://dev.mysql.com/downloads/workbench/">https://dev.mysql.com/downloads/workbench/</a>




and download Workbench for your particular operating system. Optionally signup and login, or scroll down a bit further and just start the download. After downloading, start the installation and accept all the defaults, and follow the steps below:




<ol>

 <li>After installing, run the Workbench and create a new connection by clicking on the plus icon next to the ​<em>MySQL Connections</em>​</li>

</ol>




<ol start="2">

 <li>In the ​<em>Setup New Connection</em> dialog, name your connection, e.g., cs5200-​ connection​, and use the server name, port, master username, and master password noted earlier for the ​<em>Hostname</em>​, ​<em>Port</em>​, ​<em>Username</em>​, and ​<em>Password</em>​ fields, and click ​<em>Ok</em>​.</li>

</ol>




<ol start="3">

 <li>Click on the new connection to connect to the remote MySQL database and start interacting with it through Workbench.</li>

</ol>

<h2>Create a Simple, Sample Database</h2>

This section describes creating and interacting with a simple table with some sample data. From Workbench, create a new schema name based on the course you are enrolled in, e.g., ​cs5200​.




To create a new schema, click on the ​<em>New Schema</em> button (shown at right). In the <em>new_schema</em> screen, type the name of the schema in the ​<em>Schema Name</em> field and click <em>Apply</em>​. Verify that the following command will be executed and click ​<em>Apply</em> again and then ​<em>Close</em>​ (or ​<em>Finish</em>​).




CREATE SCHEMA cs5200;




Make the new schema the default schema by double clicking it or right clicking it, and then selecting ​<em>Set as default schema</em>​.




Create a table called ​sample with a single field called ​message​. On Workbench, on the left, select the schema you created previously, and then click on the <em>New</em>​<em> Table</em> icon to create a new table (shown at right). Type ​sample for the ​<em>Name</em>​. Under the <em>Column</em>​ column, type ​<em>id</em>​, select checkboxes PK and AI, and press enter to create a primary key named ​<em>id</em>​.




Under the ​<em>id</em> field, type ​message​, select ​<em>VARCHAR(45)</em> for the <em>Datatype</em>​   ​, and click <em>Apply</em>​          ​. Verify the following command will be executed and click ​<em>Apply</em>​ again and then ​<em>Close</em>​.




CREATE TABLE cs5200.sample (   id INT NOT NULL AUTO_INCREMENT,   message VARCHAR(45) NULL,   PRIMARY KEY (id));




On the left expand the <em>Tables</em>​ ​, expand the ​<em>sample</em> table, expand the <em>Columns</em>​ ​, and verify the following fields are present: <em>id</em>​ and <em>message</em>​ ​. Insert some sample data by right clicking the <em>sample</em> table, select ​<em>Send to SQL Editor</em>​, and then <em>Insert</em>​<em> Statement</em>​. A new SQL editor window opens with the following SQL tempalate statement




<table width="625">

 <tbody>

  <tr>

   <td width="625">INSERT INTO cs5200.sample(id, message) VALUES(&lt;{id: }&gt;,&lt;{message: }&gt;);</td>

  </tr>

 </tbody>

</table>




Replace the following placeholders with the corresponding content




<table width="624">

 <tbody>

  <tr>

   <td width="224">Placeholder</td>

   <td width="400">Replace with</td>

  </tr>

  <tr>

   <td width="224">&lt;{id: }&gt;</td>

   <td width="400">1</td>

  </tr>

  <tr>

   <td width="224">&lt;{message: }&gt;</td>

   <td width="400">‘Hello Jose Annunziato’</td>

  </tr>

 </tbody>

</table>




Use YOUR NAME instead. To execute the SQL Insert command, click on the <em>Execute</em>​ button (shown on right). To verify that the content was saved, right click on the sample​ table again and select ​<em>Select Rows – Limit 1000</em>​. A new SQL editor window appears with the following SQL statement




SELECT * FROM cs5200.sample;




Verify that the results are listed below the command window similar to the following




<table width="624">

 <tbody>

  <tr>

   <td width="224">id</td>

   <td width="400">message</td>

  </tr>

  <tr>

   <td width="224">1</td>

   <td width="400">Hello Jose Annunziato</td>

  </tr>

 </tbody>

</table>




Replace the values in the <em>Insert</em>​ command to insert some more records. Use the following values:




<table width="624">

 <tbody>

  <tr>

   <td width="224">id</td>

   <td width="400">message</td>

  </tr>

  <tr>

   <td width="224">2</td>

   <td width="400">SQL is great</td>

  </tr>

  <tr>

   <td width="224">3</td>

   <td width="400">Java is awesome</td>

  </tr>

 </tbody>

</table>




Insert the new records and verify the new records exist.

<h1>Integrate Spring Boot with MySQL</h1>

Start ​<em>Spring Tool Suite</em> (STS) and open the project created in an <u>​</u><a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">earlier</a> <a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">assignment</a>​. Open the file ​application.properties under ​src/main/resources and type the following content to configure the datasource JPA will use to connect to the database




spring.datasource.url=jdbc:mysql://SERVER/SCHEMA spring.datasource.username=USERNAME spring.datasource.password=PASSWORD

spring.datasource.driver-class-name=com.mysql.jdbc.Driver spring.jpa.hibernate.ddl-auto=create spring.jpa.show-sql=true

spring.jpa.hibernate.naming-strategy=org.hibernate.cfg.ImprovedNamingStrategy spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect spring.datasource.testWhileIdle=true spring.datasource.validationQuery=SELECT 1




where ​SERVER​, SCHEMA​ ,​ USERNAME​ ,​ and ​PASSWORD above, are the server name, master username, and master password noted earlier when setting up the aws server instance. Based on the values from earlier, the three lines would be




spring.datasource.url=jdbc:mysql://​cs5200-fall2018-annunziato.cne500r o4imj.us-west-2.rds.amazonaws.com​/​cs5200_fall2018_annunziato

spring.datasource.username=jannunzi spring.datasource.password=myPa$$word123




Configure the project to download and install the MySQL Java connector library. Open the pom.xml file at the root of the project and type the following to download and install the Java MySQL library




<table width="624">

 <tbody>

  <tr>

   <td width="624">&lt;dependency&gt;&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;&lt;artifactId&gt;spring-boot-starter-data-jpa&lt;/artifactId&gt;&lt;/dependency&gt;&lt;dependency&gt;&lt;groupId&gt;mysql&lt;/groupId&gt;&lt;artifactId&gt;mysql-connector-java&lt;/artifactId&gt;&lt;version&gt;5.1.45&lt;/version&gt;&lt;/dependency&gt;</td>

  </tr>

 </tbody>

</table>




If you are using Java 9, you’ll need to add the following ​dependencies section:​




<table width="624">

 <tbody>

  <tr>

   <td width="624">&lt;dependency&gt;&lt;groupId&gt;javax.xml.bind&lt;/groupId&gt;&lt;artifactId&gt;jaxb-api&lt;/artifactId&gt;&lt;version&gt;2.3.0&lt;/version&gt; &lt;/dependency&gt;</td>

  </tr>

 </tbody>

</table>




If you are using Java 9, you’ll need to add the following in the plugins​ section:​




<table width="624">

 <tbody>

  <tr>

   <td width="624">&lt;plugin&gt;&lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;&lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;&lt;version&gt;3.2.0&lt;/version&gt; &lt;/plugin&gt;</td>

  </tr>

 </tbody>

</table>




Configure a POJO to map to an SQL table and record instances. Open the HelloObject​ .java​ file you created in an <u>​</u><a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">earlier assignment</a>​ and add the following content




<table width="624">

 <tbody>

  <tr>

   <td width="624"><strong>import javax.persistence.Entity; import javax.persistence.GeneratedValue; import javax.persistence.GenerationType; import javax.persistence.Id; </strong> <strong>@Entity(name=”hello”) </strong>public class HelloObject {<strong>@Id </strong><strong>@GeneratedValue(strategy=GenerationType.IDENTITY) </strong>private int id; public int getId() { return id;}public void setId(int id) { this.id = id;}}</td>

  </tr>

 </tbody>

</table>




The ​@Entity annotation above maps the ​HelloObject class to a table called ​hello​. All the properties in the class are mapped to fields of the same name. The ​@Id annotation configures the ​id property as the primary key. The @GeneratedValue​ annotation configures the id​ property to be generated automatically by the database, e.g., AUTO_INCREMENT​ .​ Spring Boot allows interacting with a database using the Java Persitence API (JPA) implemented through Spring’s JpaRepository​ class. JPA allows interacting with a database saving and retrieving Java object instances, each representing records in a table in the database. To use JPA you need to create your own class that extends JpaRepository and configures the Java class and its primary key type. Create a ​HelloRepository.java​ interface with the following content




package edu.neu.cs5200.controllers.hello;




import org.springframework.data.jpa.repository.JpaRepository;




public interface HelloRepository extends JpaRepository&lt;HelloObject, Integer&gt; { }




Once the ​JpaRepository has been declared, it can be used to save and retrieve records from the database. Add the following to the controller ​HelloController.java created in an <a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">earlier assignment</a><u>​</u><a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">:</a>




<table width="645">

 <tbody>

  <tr>

   <td width="645"><strong>import org.springframework.beans.factory.annotation.Autowired; </strong><strong>@RestController </strong>public class HelloController { <strong>@Autowired </strong><strong>HelloRepository helloRepository; </strong><strong> </strong><strong>@RequestMapping(“/api/hello/insert”) public HelloObject insertHelloObject() { </strong><strong>HelloObject obj = new HelloObject(“Hello Jose Annunziato!”); </strong><strong>helloRepository.save(obj); return obj; </strong><strong>} </strong>}</td>

  </tr>

 </tbody>

</table>




Replace Jose Annunziato with YOUR NAME. The above source creates a new instance of the

HelloObject with the ​message property set to the constant literal “Hello​                     Jose

Annunziato!” and then saves it to the database. In addition, let’s also create an endpoint that allows passing an arbitrary message as a parameter and then save it to the database. Add the following method to the ​HelloController class:​




<table width="624">

 <tbody>

  <tr>

   <td width="624"><strong>@RequestMapping(“/api/hello/insert/{msg}”) </strong><strong>public HelloObject insertMessage(@PathVariable(“msg”) String message) { </strong><strong>HelloObject obj = new HelloObject(message); </strong><strong>helloRepository.save(obj); return obj; </strong><strong>} </strong></td>

  </tr>

 </tbody>

</table>




The above code maps the URL pattern /api/hello/insert/{msg}​ to the insert​ Message() method. The URL contains path variable {​ msg},​ a placeholder that can be any string. The actual string value is then mapped to method parameter message​ using the @PathVariable annotation. Finally, let’s add a method to retrieve all the records from the database and return them as a ​List of ​HelloObject instances using the ​findAll() method. Add the following to method to the ​HelloController​ class:




<table width="624">

 <tbody>

  <tr>

   <td width="624"><strong>@RequestMapping(“/api/hello/select/all”) </strong><strong>public List&lt;HelloObject&gt; selectAllHelloObjects() { </strong><strong>List&lt;HelloObject&gt; hellos = </strong><strong>(List&lt;HelloObject&gt;)helloRepository.findAll(); return hellos; </strong><strong>} </strong></td>

  </tr>

 </tbody>

</table>




The above code maps the URL pattern /api/hello/select/all​ to the selectAllHello​ Objects() method. The method uses the helloRepository.findAll()​ method to retrieve all records from the hello table and converts them to a ​List of ​HelloObject instances.




Enable remote REST APIs on AWS by extending ​SpringBootServletInitializer in your SpringBootApplication class you created in an <u>​</u><a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">earlier</a> <a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">assignment</a><u>​</u><a href="https://docs.google.com/document/d/1h5gmkQcpDalbV2hUV18G_Q77y8xvxGof8N-LHw9GRPU/edit?usp=sharing">,</a> e.g., ​Cs5200Fall

2018AnnunziatoApplication​:




<table width="683">

 <tbody>

  <tr>

   <td width="683">package edu.neu.cs5200; import org.springframework.boot.SpringApplication;import org.springframework.boot.autoconfigure.SpringBootApplication; <strong>import org.springframework.boot.builder.SpringApplicationBuilder; </strong><strong>import org.springframework.boot.web.support.SpringBootServletInitializer; </strong>@SpringBootApplicationpublic class Cs5200Fall2018AnnunziatoApplication <strong>extends SpringBootServletInitializer</strong>​ { <strong>@Override </strong><strong>protected SpringApplicationBuilder configure(SpringApplicationBuilder application) { return application.sources( </strong><strong>CS5200Fall2018AnnunziatoApplication.class); </strong><strong>} </strong></td>

  </tr>

  <tr>

   <td width="683">public static void main(String[] args) {SpringApplication.run(Cs5200Fall2018AnnunziatoApplication.class, args);}}</td>

  </tr>

 </tbody>

</table>




Save all files and rebuild the project. Right click on the project and select ​<em>Maven</em> and then <em>Update Project</em>​. From the command line, navigate to the project directory and build the project using maven




mvn clean install




To test your new Webservice end point, start the Tomcat server and point your browser to




<a href="http://localhost:8080/api/hello/insert">http://localhost:8080/api/hello/insert</a>




Verify the server responds with the following JSON:




{

“id”: 1,

“message”: “Hello Jose Annunziato!”

}




Reload a couple of time and verify the ​id increments everytime you reload. List all the records by pointing your browser to ​<a href="http://localhost:8080/api/hello/select/all">http://localhost:8080/api/hello/select/all</a><u>​</u><a href="http://localhost:8080/api/hello/select/all">.</a> Verify the server responds with the following JSON:




<table width="624">

 <tbody>

  <tr>

   <td width="624">[{“id”: 1,“message”: “Hello Jose Annunziato!” },{“id”: 2,“message”: “Hello Jose Annunziato!” },{“id”: 3,“message”: “Hello Jose Annunziato!” },</td>

  </tr>

 </tbody>

</table>

]




Now try inserting some random messages, e.g., “JPA Rocks”, “Spring’s the Best”, using the insert/{msg}​ URL pattern. Point your browser to the following URLs:




<a href="http://localhost:8080/api/hello/insert/JPA%20Rocks">http://localhost:8080/api/hello/insert/JPA Rocks</a> <a href="http://localhost:8080/api/hello/insert/Spring's%20the%20Best">http://localhost:8080/api/hello/insert/Spring’s the Best</a>




List all the records by pointing your browser to <a href="http://localhost:8080/api/hello/select/all">http://localhost:8080/api/hello/select/al</a>​ <a href="http://localhost:8080/api/hello/select/all">l</a><a href="http://localhost:8080/api/hello/select/all">.</a><u>​</u> Verify the server responds with the following JSON:




<table width="624">

 <tbody>

  <tr>

   <td width="624">[{“id”: 1,“message”: “Hello Jose Annunziato!” },{“id”: 2,“message”: “Hello Jose Annunziato!” },{“id”: 3,“message”: “Hello Jose Annunziato!” },<strong>{ </strong><strong>“id”: 4, </strong><strong>“message”: “JPA Rocks” </strong><strong>}, </strong><strong>{ </strong><strong>“id”: 5, </strong><strong>“message”: “Spring’s the Best” }, </strong>]</td>

  </tr>

 </tbody>

</table>




In Workbench, verify the data has been saving to the database by right clicking on the <em>hello</em>​ table on the left, and selecting ​<em>Select Rows – Limit 1000</em>​. A new SQL editor window appears with the following SQL statement




SELECT * FROM cs5200.hello;




Verify that the results are listed below the command window similar to the following:




<table width="624">

 <tbody>

  <tr>

   <td width="224">id</td>

   <td width="400">message</td>

  </tr>

  <tr>

   <td width="224">1</td>

   <td width="400">Hello Jose Annunziato!</td>

  </tr>

  <tr>

   <td width="224">2</td>

   <td width="400">Hello Jose Annunziato!</td>

  </tr>

  <tr>

   <td width="224">3</td>

   <td width="400">Hello Jose Annunziato!</td>

  </tr>

  <tr>

   <td width="224">4</td>

   <td width="400">JPA Rocks</td>

  </tr>

  <tr>

   <td width="224">5</td>

   <td width="400">Spring’s the Best</td>

  </tr>

 </tbody>

</table>




Where the messages should include YOUR NAME instead. Redeploy the war file to the remote Web application deployed on AWS and verify the links work there as well. The links should look similiar to the following:




http://cs5200-fall2018-annunziato.us-west-2.elasticbeanstalk.com/api/hello/insert http://cs5200-fall2018-annunziato.us-west-2.elasticbeanstalk.com/api/hello/insert/Some parameterized message

http://cs5200-fall2018-annunziato.us-west-2.elasticbeanstalk.com/api/hello/select/all




Note that when the application recompiles and/or redeploys, the tables are recreated and previous data is lost. This is fine for now while we are getting started in the early stages of development and we have not settled on a data model and the schema needs to reflect changes in the data model. When we settle on a data model, in later assignments, we will reconfigure JPA to not recreate the tables and loose our data.

<h1>Deliverables</h1>

As a deliverable add a section called “Integrating Spring Boot with MySQL” to your Submission.md file that contains three links to the Web service endpoints created in this assignment. The links should return JSON data similar in structure and content as verified throughout the assignment. The ​Submission.md​ should look as follows:

Integrating Spring Boot with MySQL

<a href="http://localhost:8080/api/hello/insert">Insert a static hello message</a>

<a href="http://localhost:8080/api/hello/insert/Some%20parameterized%20message">Insert a parameterized hello message</a>

<a href="http://localhost:8080/api/hello/select/all">Retrieve all hello messages</a>

Add, commit and push all the work completed under the Project-2​ directory, to your course repository setup in an earlier assignment. Use the following message for your commit:

“Integrating Spring Boot with MySQL”​.