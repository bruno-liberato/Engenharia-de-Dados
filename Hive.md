| 1 - INSTAÇÃO DO JAVA |
| -------------------- |

## 

### Etapa I

```shell
wget --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" https://javadl.oracle.com/webapps/download/GetFile/1.8.0_281-b09/89d678f2be164786b292527658ca1605/linux-i586/jdk-8u281-linux-x64.tar.gz
```

### Etapa II

```shell
tar -zxvf jdk-8u281-linux-x64.tar.gz
```

### Etapa III

```shell
sudo mv jdk1.8.0_281/ /opt/jdk
```

### Etapa IV

Para configurar as variáveis **PATH** e **JAVA_HOME**, adicione os seguintes comandos ao arquivo **~/.bashrc.**

```shell
sudo nano ~/.bashrc
```

### Etapa V

Incluir no final do arquivo as variárias de ambiente do JAVA.

```SHELL
# JAVA JDK
export JAVA_HOME=/opt/jdk
export JRE_HOME=/opt/jdk/jre
export PATH=$PATH:$JAVA_HOME/bin
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```

Agora aplique todas as alterações no sistema em execução atual.

```shell
source ~/.bashrc
```

### 2 - INSTALAÇÃO DO HIVE

### **Etapa I**

```SHELL
wget https://downloads.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gzshe
```

No download bem-sucedido, você verá a seguinte resposta:

```shell
apache-hive-3.1.3-bin.tar.gz
```

## Etapa II

As etapas a seguir são necessárias para instalar o Hive em seu sistema.

### Etapa III

O comando a seguir é usado para verificar o download e extrair o arquivo hive:

```shell
tar -zxvf apache-hive-3.1.3-bin.tar.gz
```

### Etapa IV

Precisamos copiar os arquivos do superusuário “su -”. Os comandos a seguir são usados para copiar os arquivos do diretório extraído para o diretório /usr/local/hive”.

```shell
sudo mv apache-hive-3.1.3-bin /opt/hive
```

### Etapa IV 

Configurando o ambiente para o Hive

Você pode configurar o ambiente Hive anexando as seguintes linhas ao arquivo **~/.bashrc** :

```shell
sudo nano ~/.bashrc
```

```shell
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=$CLASSPATH:/usr/local/Hadoop/lib/*:.
export CLASSPATH=$CLASSPATH:/usr/local/hive/lib/*:.
```

O comando a seguir é usado para executar o arquivo ~/.bashrc.

```shell
$ source ~/.bashrc
```

## Etapa V

Para configurar o Hive com o Hadoop, você precisa editar o arquivo **hive-env.sh**. Os comandos a seguir redirecionam para a pasta de **configuração** do Hive e copiam o arquivo de modelo:

```shell
cp hive-env.sh.template hive-env.sh
```

Edite o arquivo **hive-env.sh** anexando a seguinte linha:

```shell
export HADOOP_HOME=/usr/local/hadoop
```

 

***Agora você precisa de um servidor de banco de dados externo para configurar o Metastore. Usamos o banco de dados (SGBD) relacional.***



## Etapa VI

Configurar o Metastore significa especificar para o Hive onde o banco de dados está armazenado. Você pode fazer isso editando o arquivo hive-site.xml, que está no diretório $HIVE_HOME/conf. Antes de tudo, copie o arquivo de modelo usando o seguinte comando:

```
$ cd $HIVE_HOME/conf
$ cp hive-default.xml.template hive-site.xml
```

Edite **hive-site.xml** e anexe as seguintes linhas entre as tags <configuration> e </configuration>:

```
<property>
   <name>javax.jdo.option.ConnectionURL</name>
   <value>jdbc:derby://localhost:1527/metastore_db;create=true </value>
   <description>JDBC connect string for a JDBC metastore </description>
</property>
```

Crie um arquivo chamado jpox.properties e adicione as seguintes linhas nele:

```
javax.jdo.PersistenceManagerFactoryClass =

org.jpox.PersistenceManagerFactoryImpl
org.jpox.autoCreateSchema = false
org.jpox.validateTables = false
org.jpox.validateColumns = false
org.jpox.validateConstraints = false
org.jpox.storeManagerType = rdbms
org.jpox.autoCreateSchema = true
org.jpox.autoStartMechanismMode = checked
org.jpox.transactionIsolation = read_committed
javax.jdo.option.DetachAllOnCommit = true
javax.jdo.option.NontransactionalRead = true
javax.jdo.option.ConnectionDriverName = org.apache.derby.jdbc.ClientDriver
javax.jdo.option.ConnectionURL = jdbc:derby://hadoop1:1527/metastore_db;create = true
javax.jdo.option.ConnectionUserName = APP
javax.jdo.option.ConnectionPassword = mine
```

## Etapa VII 

Antes de executar o Hive, você precisa criar a pasta **/tmp** e uma pasta Hive separada no HDFS. Aqui, usamos a pasta **/user/hive/warehouse .** Você precisa definir a permissão de gravação para essas pastas recém-criadas, conforme mostrado abaixo:

```
chmod g+w
```

Agora defina-os no HDFS antes de verificar o Hive. Use os seguintes comandos:

```
$ $HADOOP_HOME/bin/hadoop fs -mkdir /tmp 
$ $HADOOP_HOME/bin/hadoop fs -mkdir /user/hive/warehouse
$ $HADOOP_HOME/bin/hadoop fs -chmod g+w /tmp 
$ $HADOOP_HOME/bin/hadoop fs -chmod g+w /user/hive/warehouse
```

Os comandos a seguir são usados para verificar a instalação do Hive:

```
$ cd $HIVE_HOME
$ bin/hive
```

Na instalação bem-sucedida do Hive, você verá a seguinte resposta:

```
Logging initialized using configuration in jar:file:/home/hadoop/hive-0.9.0/lib/hive-common-0.9.0.jar!/hive-log4j.properties 
Hive history file=/tmp/hadoop/hive_job_log_hadoop_201312121621_1494929084.txt
………………….
hive>
```

O seguinte comando de amostra é executado para exibir todas as tabelas:

```
hive> show tables; 
OK 
Time taken: 2.798 seconds 
hive>
```
