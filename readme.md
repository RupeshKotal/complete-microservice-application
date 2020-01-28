# What is microservice?
Microservice is modern as well as a popular architectural term for designing software application over the last few years.  
There are lots of definition on the internet to describe what microservice really is and all of them are correct.  
But wanna describe it simply, concisely and more importance is exactly.  

A microservice application is consist of different services where every service is an application and  
  1. Independently deployable   
  2. Independently scalable  

above two are the key requirements of a microservice application.

In this microservice application here are two service product-service and offer-service 
both independently deployable and scaleable.  
They are also using a different database but this is not an issue about microservice architecture. They can use the same database.  



# Run the services

## Prerequisites
### 1. Java 8 and Maven
Java 8 and Maven should be installed on your system and configured with your IDE.

### 2. RabbitMQ
RabbitMQ should be installed and running in your system where product service will be deployed.  
Though this example is for localhost you need to install in your local computer.

#### Install RabbitMQ  
##### [Mac OS]  
Here is **install-rabbitmq-macOS.sh** file to install RabbitMQ type `bash install-rabbitmq-macOS.sh` in your system terminal.  
then start RabbitMQ by typing `rabbitmq-server`.

##### [Ubuntu OS]
Check this for [Installing on Debian and Ubuntu](https://www.rabbitmq.com/install-debian.html)

##### [Windows]  
Check this for [Installing on Windows](https://www.rabbitmq.com/install-windows.html#installer)

### 3. Lombok
Lombok plugin should be installed in you IDE otherwise IDE will catch code error.

## Run Product service
It's a spring boot maven application that's why If maven is installed and configured to your system, application can be run in terminal by
````
cd product-service/
mvn clean install
mvn spring-boot:run
````
Otherwise configure your system and open it on your favourite IDE to run.  
This application will start on port **8081**

Access it's data source console in browser by
`localhost:8081/h2`  
To connect product data source h2 console use below credentials   
JDBC URL  : `jdbc:h2:~/product`  
User Name : `root`  
Password  : `root`  

Check product table. Right now there is no products.  
Let's add a new product by calling `localhost:8081/products` POST endpoint with below body
````
{
	"productCode": "ABC1",
	"productTitle": "watch",
	"imageUrl": "watch.img"
}
````
Now check product table and there is a product with product_id is 1 [remember it] and no **discount_offer**.   
We will add it's offer by a event from other application [offer-service]

## Run Offer service
It's also a spring boot maven application and run it similarly as product-service.  
It will run on port **8082**.

Access it's data source console in browser by
`localhost:8082/h2`  
To connect offer data source h2 console use below credentials  
JDBC URL  : `jdbc:h2:~/offer`  
User Name : `root`  
Password  : `root`

Check offer table and there is no offer right now.  
Let's add a offer for product_id=1 by calling `localhost:8082/offer` POST endpoint with below body
````
{
	"productId": 1,
	"discountOffer": 30
}
````
Now check offer table there is a offer recorded added for product_id=1
and product table is updated with 30% discount_offer for product_id=1.

**Note:** Here Offer and Product table are from different data source and running on different port.

## How it's working?
When we add an offer for a product from **offer-service** it pushes an event notification to **product-service** with
discount_offer and **product-service** update itself by its business logic.