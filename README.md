# ms-spring-cloud-eureka-RegistrationService
microservice d'enregistrement spring cloud eureka

1-registry/discover project:
   a-POM:
      </dependencies>
         <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
          </dependency>
        </dependencies>

        <dependencyManagement>
          <dependencies>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-cloud-dependencies</artifactId>
                  <version>${spring-cloud.version}</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-dependencies</artifactId>
                  <version>${spring.cloud-version}</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
          </dependencies>
        </dependencyManagement>
        
   b-main => @EnableEurekaServer
   
   c-bootstrap.properties:
       eureka.client.register-with-eureka=false
       eureka.client.fetch-registry=false

      logging.level.com.netflix.eureka=OFF
      logging.level.com.netflix.discovery=OFF
      eureka.server.renewalPercentThreshold=0.33
      eureka.client.serviceUrl.defaultZone= http://localhost:8761/eureka/
   
   
2- in clinet microservice:
    a-POM:
      <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
            <version>4.0.0</version>
        </dependency>
    b-main: 
       @EnableDiscoveryClient
       //@EnableEurekaClient
      
    c-bootstrap.properties:
        eureka.client.serviceUrl.defaultZone= http://localhost:8761/eureka/
        eureka.client.register-with-eureka=false
        eureka.client.fetch-registry=false
    d-config
         @Configuration
         public class invoiceConfig {
             @Bean
             /*
                 avoid confusion in case there are several instances of a microservice
                 if 2 instance is active,"httprequest"  wil call the first instance in first time
                 if smae "httprequest" called, the second instance of this microservice will be called
              */
             @LoadBalanced
             public RestTemplate restTemplate(){
                 return new RestTemplate();
             }
          }
