# MicroServicesApp


## Versions
- 2.2.0 M1.RELEASE
- Finchley.M8

## URLs

|     Application       |     URL          |
| ------------- | ------------- |
| Limits Service | http://localhost:8080/limits|
|Spring Cloud Config Server| http://localhost:8888/limits-service/default http://localhost:8888/limits-service/dev|
|  Currency Converter Service - Direct Call| http://localhost:8100/currency-converter/from/USD/to/INR//usr/local/var/log/rabbitmq/rabbit@localhost.log/usr/local/var/log/rabbitmq/rabbit@localhost.logquantity/10|
|  Currency Converter Service - Feign| http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000|
| Currency Exchange Service | http://localhost:8000/currency-exchange/from/EUR/to/INR http://localhost:8001/currency-exchange/from/USD/to/INR|
| Eureka | http://localhost:8761/|
| Zuul - Currency Exchange & Exchange Services | http://localhost:8765/currency-exchange-service/currency-exchange/from/EUR/to/INR http://localhost:8765/currency-conversion-service/currency-converter-feign/from/USD/to/INR/quantity/10|

## VM Argument

-Dserver.port=8001

## Commands
- mkdir git-configuration-repo
- cd git-configuration-repo/
- git init
- git add -A
- git commit -m "first commit"

## Ports

|     Application       |     Port          |
| ------------- | ------------- |
| Limits Service | 8080, 8081, ... |
| Spring Cloud Config Server | 8888 |
|  |  |
| Currency Exchange Service | 8000, 8001, 8002, ..  |
| Currency Conversion Service | 8100, 8101, 8102, ... |
| Netflix Eureka Naming Server | 8761 |
| Netflix Zuul API Gateway Server | 8765 |
| Zipkin Distributed Tracing Server | 9411 |


## Installing Tools
Eclipse & Embedded Maven
PostMan
Git Client - https://git-scm.com/
Rabbit MQ - https://www.rabbitmq.com/download.html
Installing Eclipse & Embedded Maven
Installation Video : https://www.youtube.com/playlist?list=PLBBog2r6uMCSmMVTW_QmDLyASBvovyAO3
GIT Repository For Installation : https://github.com/in28minutes/getting-started-in-5-steps
PDF : https://github.com/in28minutes/SpringIn28Minutes/blob/master/InstallationGuide-JavaEclipseAndMaven_v2.pdf
Installing Rabbit MQ
Windows
https://www.rabbitmq.com/install-windows.html
https://www.rabbitmq.com/which-erlang.html
http://www.erlang.org/downloads
Video - https://www.youtube.com/watch?v=gKzKUmtOwR4
Mac
https://www.rabbitmq.com/install-homebrew.html

### Diagrams

- http://viz-js.com/
- http://dreampuf.github.io/GraphvizOnline/

```


MicroserviceCommunication
~~~~~~~~~~~~~~~~~~~~~~~~~
digraph architecture {
  rankdir=TB;
{rank=same; MovieService, CustomerService, ReviewService, BookingService, FareCalculationService};
DB1,DB2,DB3,DB4,DB5[shape=cylinder]
MovieService, CustomerService, ReviewService, BookingService, FareCalculationService[shape=component]

  MovieService->DB1;
  CustomerService->DB2;
ReviewService->DB3;
BookingService ->DB4;
FareCalculationService ->DB5;


}

digraph architecture {
  rankdir=TB;
{rank=same; MovieApplication};
LARGEDB[shape=cylinder];
  MovieApplication->LARGEDB;

}

digraph architecture {
  rankdir=TB;
{rank=same; CurrencyCalculationService, CurrencyExchangeService, LimitsService};
Configuration[shape=cylinder]
Database[shape=cylinder]
LimitsService, CurrencyCalculationService, CurrencyExchangeService[shape=component]

  CurrencyCalculationService -> CurrencyExchangeService -> LimitsService;
  
  CurrencyExchangeService->Database;
  LimitsService->Configuration;

}

Microservices-Environments
~~~~~~~~~~~~~~~~~~~~~~~~~~~`
digraph architecture {
  rankdir=LR;

node[shape=record]
LimitsService, CurrencyCalculationService, CurrencyExchangeService[shape=component]

  CurrencyCalculationService -> CurrencyExchangeService -> LimitsService

  subgraph CurrencyCalculationService {
      {rank=same; CurrencyCalculationService,CCDEV,CCQA,CCSTAGE, CCPROD};
  }
 
  subgraph CurrencyExchangeService {
      CurrencyExchangeService;
      {rank=same; CurrencyExchangeService,CEDEV,CEQA, CESTAGE, CEPROD};
  }
  
  subgraph LimitsService {
      CurrencyExchangeService;
      {rank=same; LimitsService,LSDEV,LSQA, LSSTAGE, LSPROD};
  }
 
}


SpringCloudConfigServer
#######################
digraph architecture {
rankdir = TB;
node[shape=component]
Git[shape=cylinder]
{rank=same; CurrencyCalculationService, CurrencyExchangeService, LimitsService};
CurrencyExchangeService -> SpringCloudConfigServer;
CurrencyCalculationService -> SpringCloudConfigServer;
LimitsService -> SpringCloudConfigServer
SpringCloudConfigServer -> Git

}

EUREKANAMINGSERVER
#######################
digraph architecture {
rankdir = TB;
node[shape=component]
{rank=same; CurrencyCalculationService, CurrencyExchangeService, LimitsService};
CurrencyExchangeService -> EurekaNamingServer;
CurrencyCalculationService -> EurekaNamingServer;
LimitsService -> EurekaNamingServer
}

RibbonClientSideLoadBalancing
#######################
digraph architecture {
rankdir = TB;
node[shape=component]
Ribbon[shape=underline]

{rank=same; CurrencyExchangeService1, CurrencyExchangeService2, CurrencyExchangeService3};
{rank=same; Ribbon, NamingServer };
Ribbon -> CurrencyExchangeService1
Ribbon -> CurrencyExchangeService2
Ribbon -> CurrencyExchangeService3
CurrencyCalculationService -> Ribbon
Ribbon -> NamingServer

}

ZipkinDistributedTracingServer
###############################
digraph architecture {
rankdir = TB;
node[shape=component]
Database[shape=cylinder]
RabbitMQ[shape=underline]
{rank=same; CurrencyCalculationService, CurrencyExchangeService, LimitsService};
CurrencyExchangeService -> RabbitMQ
CurrencyCalculationService -> RabbitMQ;
LimitsService -> RabbitMQ
RabbitMQ -> ZipkinDistributedTracingServer
ZipkinDistributedTracingServer -> Database

}

CurrencyExchangeMicroserviceDeployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~``
digraph architecture {
rankdir = TB;
node[shape=record]
{rank=same; DEV, QA, STAGE, PROD};

DEV -> DEV1

QA -> QA1
QA -> QA2

STAGE -> STAGE1

PROD -> PROD1
PROD -> PROD2
PROD -> PROD3
PROD -> PROD4

}

CurrencyConversionMicroserviceDeployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~``
digraph architecture {
rankdir = TB;
node[shape=record]
{rank=same; DEV, QA, STAGE, PROD};

DEV -> DEV1

QA -> QA1
QA -> QA2

STAGE -> STAGE1

PROD -> PROD1

}


```
