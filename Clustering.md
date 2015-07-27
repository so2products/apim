
Clustering

1)	Change Offsets
Publisher 	-0
Store		-1
Key Manager	-2
Gateway	-3
2)	Change axis2.xml of publisher & Store(repository\conf\axis2\axis2.xml)

•	Enable clustering
<clustering class="org.wso2.carbon.core.clustering.hazelcast.HazelcastClusteringAgent" enable="true">

•	Change the MembershipScheme
<parameter name="membershipScheme">wka</parameter>

•	Add Local Member Host
<parameter name="localMemberHost">172.22.217.25</parameter>
		
•	Add Local Member Port
<parameter name="localMemberPort">4000</parameter>

•	Add members

<members>
            <member>
                <hostName>127.0.0.1</hostName>
                <port>4000</port>
            </member>
</members>

3)	Change api-manager.xml of Publisher & Store

•	Disable ThriftServer
<EnableThriftServer>false</EnableThriftServer>

4)	Change api-manager.xml of Key manager
•	Enable Thrift Server
<EnableThriftServer>true</EnableThriftServer>

•	Set Thift server host
<ThriftServerHost>localhost</ThriftServerHost>


5)	Create databases
AMDB 		– API Manager Database
User_DB	- User Database
REG_DB	- Registry Database

4) Add database connector jar file (ex. mysql-connector-java-5.1.6.jar) to repository\components\lib folder in publisher, store and keymanager
5) Change master-datasources.xml file and add datasources in publisher, store & Key manager (Location-C:\Software\Clustering\Publisher\repository\conf\datasources) 
Publisher- UserDB, RegDB and AMDB
Store	- UserDB, RegDB and AMDB
Key Manager- UserDB and RegDB
 
6)	Change the api-manager.xml in repository\conf 
•	Change the datasource name
<DataSourceName>jdbc/WSO2AM_DB</DataSourceName>

7)	Change the registry.xml in repository\conf 
•	Add data source details
 

8)	Change the usr-mgt.xml in repository\conf
•	Change the dataSource property in User Manager tag
<Property name="dataSource">jdbc/WSO2CarbonDB</Property>

9)	Change the api-manager.xml in Publisher & Store
•	Add ServerURL of auth manager tag (Add the url of key manager)
<ServerURL>https://localhost:9445/services/</ServerURL>

•	Add ServerURL and gateway end point of API Gateway tag(All the url of Gateway)
<ServerURL>https://localhost:9446/services/</ServerURL>
<GatewayEndpoint>http://localhost:8283,https://localhost:8246</GatewayEndpoint>
•	Add server url of APIKeyValidator tag(Add the url of key manager)
<ServerURL>https://localhost:9445/services/</ServerURL>

10)	Change the api_manager.xml in Key Manager
•	Add Revoke API URL tag
<RevokeAPIURL>https://localhost:8246/revoke</RevokeAPIURL>

11)	Change api-manager.xml in Gateway
•	Add server url of APIKeyValidator tag
<ServerURL>https://localhost:9445/services/</ServerURL>

12)	Change endpoint in _AuthorizeAPI_.xml of Gateway(repository\deployment\server\synapse-configs\default\api)
•	https://localhost:9445/oauth2/authorize
13)	Change endpoint in _RevokeAPI_.xml of Gateway (repository\deployment\server\synapse-configs\default\api)
•	https://localhost:9445/oauth2/revoke
14)	Change endpoint in _TokenAPI_.xml of Gateway (repository\deployment\server\synapse-configs\default\api)
•	https://localhost:9445/oauth2/token
