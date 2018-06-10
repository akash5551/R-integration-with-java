# R-integration-with-java
Using Rserve  how to make TCP/IP  connection to run r code located on remote machine
## install Rserve package into Rstudio
```markdown
install.packages('Rserve')
```
## it will download some java jar in the package directory
_directory structure will be soething like C:\Users\username\Documents\R\win-library\3.4\Rserve\java_
it will contain two jar
* Rengine.jar
* Rserver.jar
## create a java Web project in the IDE of your choice ex-eclipse
### craete project in IDE
![create project](/images/create1.png)
### select dynamic project
![select Dynamic web project](/images/create2.png)
### give a project name
![give project name](/images/create3.png)

## copy the R and Java integration jar to the project
### copy the jar from the java directory of Rserver package
![create project](/images/copy1.png)
### paste the jar to the Web-lib
![select Dynamic web project](/images/copy2.png)
![select Dynamic web project](/images/copy3.png)
### the web lib looks like this
![give project name](/images/copy4.png)

## create a java class 
### give class name RIntegration
![create project](/images/class2.png)
## Methods to invoke
### to connect to remote machine
_it will take four parameters host machine name,port no default 6312,username,password to connect to the machine in which you want to run R code_
```markdown
public static RConnection connection = null; 
	public static RConnection open(RConnection connection,String hostname,int port,String username,String password) throws RserveException{
        connection = new RConnection(hostname,port);
        connection.login(username,password);
        return connection;
	}
```
### if you want to connect to the same server
```markdown
	public static RConnection open(RConnection connection) throws RserveException{
		connection = new RConnection();
		return connection;
	}
```
### how to run scripts we need to assign a path like a central repo
_to run code on windows it will take four \\\\ in the path_
```markdown
public static String scriptPAth="put the path which you want to pass";//in the src variable
public static void scriptloading(RConnection connection,String filename) throws RserveException{
		String path= "source('C:\\\\Users\\\\guptakas\\\\Rscripts\\\\"+filename+"')";
		 connection.eval(path);
	}
```
### to run your code you have to define custom methods
_here is a function to add_
```markdown
	public static REXP funcall(RConnection connection,String funcname,int num1,int num2) throws RserveException, REXPMismatchException{
		return connection.eval(funcname+"("+num1+","+num2+")");
	}
```
### to close a conection
```markdown
public static void close(RConnection connection) throws RserveException{
		if(connection !=null){
			connection.close();
		}
	}
```
### to run a code remotely
* Rserve.exe should be running 
* to run Rserve.exe
library('Rserve')
Rserve()
* it will acts as a listner

### a method for synchronusly running the scripts
* connect 
```markdown
connection = open(connection,"Ip addres",6311,"username", "password");
```
* load the scripts it will take scriptname
```markdown
scriptloading(connection,"add.R");
```
* contents of add.R
_similary defines your own mwthod_
```markdown
myAdd=function(x,y){
    sum=x+y
    return(sum)
}
```
* custom method to run code
```markdown
int sum=funcall(connection,"myAdd",10,20).asInteger();
```
* print or return it or consume the way you want
```markdown
System.out.println("The sum is=" + sum);
```
#### the method will look something like this
```markdown
public static void test1() throws RserveException {
		System.out.println("entering");
	    try {
	        /* Create a connection to Rserve instance running
	         * on default port 6311
	         */
	    	
	        //connection = new RConnection("127.0.0.1",6311);
	        //connection.login("guptakas", "Microfocus@123");
	    	connection = open(connection,"Ip addres",6311,"username", "password");
	    	System.out.println("connection established");
	        /* Note four slashes (\\\\) in the path */
	        scriptloading(connection,"add.R");
	        int sum=funcall(connection,"myAdd",10,20).asInteger();
	        System.out.println("The sum is=" + sum);
	    } catch (RserveException e) {
	        e.printStackTrace();
	    } catch (REXPMismatchException e) {
	        e.printStackTrace();
	    }finally{
	        close(connection);
	        if(connection.isConnected())
	        {
	        	System.out.println("still connected");
	        }
	        System.out.println("connection close");
	    }
	    
	}
```
### 
```markdown
```

## 
```markdown
```
## 
```markdown
```
## 
```markdown
```
## 
```markdown
```
