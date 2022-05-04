Todos os subprojetos do Hadoop, como Hive, Pig e HBase, suportam o sistema operacional Linux. Portanto, você precisa instalar qualquer sistema operacional com sabor Linux. As etapas simples a seguir são executadas para a instalação do Hive:

## Etapa 1: Verificando a instalação JAVA

O Java deve ser instalado em seu sistema antes de instalar o Hive. Vamos verificar a instalação do java usando o seguinte comando:

```
$ java –version
```

Se o Java já estiver instalado em seu sistema, você verá a seguinte resposta:

```
java version "1.7.0_71" 
Java(TM) SE Runtime Environment (build 1.7.0_71-b13) 
Java HotSpot(TM) Client VM (build 25.0-b02, mixed mode)
```

Se o java não estiver instalado em seu sistema, siga as etapas abaixo para instalar o java.

## Instalando o Java

### Etapa I:

Baixe o java (JDK <última versão> - X64.tar.gz) visitando o seguinte link [http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html.](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

Em seguida, o jdk-7u71-linux-x64.tar.gz será baixado em seu sistema.

### Etapa II:

Geralmente, você encontrará o arquivo java baixado na pasta Downloads. Verifique-o e extraia o arquivo jdk-7u71-linux-x64.gz usando os comandos a seguir.

```
$ cd Downloads/
$ ls
jdk-7u71-linux-x64.gz
$ tar zxf jdk-7u71-linux-x64.gz
$ ls
jdk1.7.0_71 jdk-7u71-linux-x64.gz
```

### Etapa III:

Para disponibilizar o java para todos os usuários, você deve movê-lo para o local “/usr/local/”. Abra o root e digite os comandos a seguir.

```
$ su
password:
# mv jdk1.7.0_71 /usr/local/
# exit
```

### Etapa IV:

Para configurar as variáveis PATH e JAVA_HOME, adicione os seguintes comandos ao arquivo ~/.bashrc.

```
export JAVA_HOME=/usr/local/jdk1.7.0_71
export PATH=$PATH:$JAVA_HOME/bin
```

Agora aplique todas as alterações no sistema em execução atual.

```
$ source ~/.bashrc
```

### Etapa V:

Use os seguintes comandos para configurar alternativas java:

```
# alternatives --install /usr/bin/java/java/usr/local/java/bin/java 2

# alternatives --install /usr/bin/javac/javac/usr/local/java/bin/javac 2

# alternatives --install /usr/bin/jar/jar/usr/local/java/bin/jar 2

# alternatives --set java/usr/local/java/bin/java

# alternatives --set javac/usr/local/java/bin/javac

# alternatives --set jar/usr/local/java/bin/jar
```

Agora verifique a instalação usando o comando java -version do terminal conforme explicado acima.

## Etapa 2: Verificando a instalação do Hadoop

O Hadoop deve ser instalado em seu sistema antes de instalar o Hive. Vamos verificar a instalação do Hadoop usando o seguinte comando:

```
$ hadoop version
```

Se o Hadoop já estiver instalado em seu sistema, você receberá a seguinte resposta:

```
Hadoop 2.4.1 Subversion https://svn.apache.org/repos/asf/hadoop/common -r 1529768 
Compiled by hortonmu on 2013-10-07T06:28Z 
Compiled with protoc 2.5.0 
From source with checksum 79e53ce7994d1628b240f09af91e1af4
```

Se o Hadoop não estiver instalado em seu sistema, prossiga com as seguintes etapas:

## Baixando o Hadoop

Baixe e extraia o Hadoop 2.4.1 da Apache Software Foundation usando os comandos a seguir.

```
$ su
password:
# cd /usr/local
# wget http://apache.claz.org/hadoop/common/hadoop-2.4.1/
hadoop-2.4.1.tar.gz
# tar xzf hadoop-2.4.1.tar.gz
# mv hadoop-2.4.1/* to hadoop/
# exit
```

## Instalando o Hadoop no Modo Pseudo Distribuído

As etapas a seguir são usadas para instalar o Hadoop 2.4.1 no modo pseudodistribuído.

### Etapa I: Configurando o Hadoop

Você pode definir variáveis de ambiente do Hadoop anexando os seguintes comandos ao arquivo **~/.bashrc** .

```
export HADOOP_HOME=/usr/local/hadoop 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export HADOOP_COMMON_HOME=$HADOOP_HOME 
export HADOOP_HDFS_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native export
PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
```

Agora aplique todas as alterações no sistema em execução atual.

```
$ source ~/.bashrc
```

### Etapa II: Configuração do Hadoop

Você pode encontrar todos os arquivos de configuração do Hadoop no local “$HADOOP_HOME/etc/hadoop”. Você precisa fazer as alterações adequadas nesses arquivos de configuração de acordo com sua infraestrutura do Hadoop.

```
$ cd $HADOOP_HOME/etc/hadoop
```

Para desenvolver programas Hadoop usando java, você precisa redefinir as variáveis de ambiente java no arquivo **hadoop-env.sh** substituindo o valor **JAVA_HOME** pelo local do java em seu sistema.

```
export JAVA_HOME=/usr/local/jdk1.7.0_71
```

Abaixo está a lista de arquivos que você precisa editar para configurar o Hadoop.

**core-site.xml**

O arquivo **core-site.xml** contém informações como o número da porta usada para a instância do Hadoop, memória alocada para o sistema de arquivos, limite de memória para armazenar os dados e o tamanho dos buffers de leitura/gravação.

Abra o core-site.xml e adicione as seguintes propriedades entre as tags <configuration> e </configuration>.

```
<configuration>

   <property> 
      <name>fs.default.name</name> 
      <value>hdfs://localhost:9000</value> 
   </property>
   
</configuration>
```

**hdfs-site.xml**

O arquivo **hdfs-site.xml** contém informações como o valor dos dados de replicação, o caminho do namenode e o caminho do datanode de seus sistemas de arquivos locais. Significa o local onde você deseja armazenar a infra do Hadoop.

Suponhamos os seguintes dados.

```
dfs.replication (data replication value) = 1

(In the following path /hadoop/ is the user name.
hadoopinfra/hdfs/namenode is the directory created by hdfs file system.)

namenode path = //home/hadoop/hadoopinfra/hdfs/namenode

(hadoopinfra/hdfs/datanode is the directory created by hdfs file system.)
datanode path = //home/hadoop/hadoopinfra/hdfs/datanode
```

Abra este arquivo e adicione as seguintes propriedades entre as tags <configuration>, </configuration> neste arquivo.

```
<configuration>

   <property> 
      <name>dfs.replication</name> 
      <value>1</value> 
   </property> 
   <property> 
      <name>dfs.name.dir</name> 
      <value>file:///home/hadoop/hadoopinfra/hdfs/namenode </value> 
   </property> 
   <property> 
      <name>dfs.data.dir</name>
      <value>file:///home/hadoop/hadoopinfra/hdfs/datanode </value > 
   </property>
   
</configuration>
```

**Nota:** No arquivo acima, todos os valores de propriedade são definidos pelo usuário e você pode fazer alterações de acordo com sua infraestrutura Hadoop.

**fio-site.xml**

Este arquivo é usado para configurar o yarn no Hadoop. Abra o arquivo yarn-site.xml e adicione as seguintes propriedades entre as tags <configuration>, </configuration> neste arquivo.

```
<configuration>

   <property> 
      <name>yarn.nodemanager.aux-services</name> 
      <value>mapreduce_shuffle</value> 
   </property>
   
</configuration>
```

**site mapred.xml**

Este arquivo é usado para especificar qual framework MapReduce estamos usando. Por padrão, o Hadoop contém um modelo de yarn-site.xml. Em primeiro lugar, você precisa copiar o arquivo do mapred-site,xml.template para o arquivo mapred-site.xml usando o comando a seguir.

```
$ cp mapred-site.xml.template mapred-site.xml
```

Abra o arquivo **mapred-site.xml** e inclua as seguintes propriedades entre as tags <configuration>, </configuration> neste arquivo.

```
<configuration>

   <property> 
      <name>mapreduce.framework.name</name> 
      <value>yarn</value> 
   </property>

</configuration>
```

## Verificando a instalação do Hadoop

As etapas a seguir são usadas para verificar a instalação do Hadoop.

### Etapa I: Configuração do nó de nome

Configure o namenode usando o comando “hdfs namenode -format” da seguinte forma.

```
$ cd ~
$ hdfs namenode -format
```

O resultado esperado é o seguinte.

```
10/24/14 21:30:55 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************ 
STARTUP_MSG: Starting NameNode 
STARTUP_MSG: host = localhost/192.168.1.11 
STARTUP_MSG: args = [-format] 
STARTUP_MSG: version = 2.4.1 
... 
... 
10/24/14 21:30:56 INFO common.Storage: Storage directory 
/home/hadoop/hadoopinfra/hdfs/namenode has been successfully formatted. 
10/24/14 21:30:56 INFO namenode.NNStorageRetentionManager: Going to 
retain 1 images with txid >= 0 
10/24/14 21:30:56 INFO util.ExitUtil: Exiting with status 0
10/24/14 21:30:56 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************ 
SHUTDOWN_MSG: Shutting down NameNode at localhost/192.168.1.11
 ************************************************************/
```

### Etapa II: Verificando o Hadoop dfs

O comando a seguir é usado para iniciar o dfs. A execução deste comando iniciará seu sistema de arquivos Hadoop.

```
$ start-dfs.sh
```

A saída esperada é a seguinte:

```
10/24/14 21:37:56 
Starting namenodes on [localhost] 
localhost: starting namenode, logging to /home/hadoop/hadoop-2.4.1/logs/hadoop-hadoop-namenode-localhost.out 
localhost: starting datanode, logging to /home/hadoop/hadoop-2.4.1/logs/hadoop-hadoop-datanode-localhost.out 
Starting secondary namenodes [0.0.0.0]
```

### Etapa III: Verificando o Script de Fios

O comando a seguir é usado para iniciar o script yarn. A execução deste comando iniciará seus daemons de fios.

```
$ start-yarn.sh
```

A saída esperada é a seguinte:

```
starting yarn daemons 
starting resourcemanager, logging to /home/hadoop/hadoop-2.4.1/logs/yarn-hadoop-resourcemanager-localhost.out 
localhost: starting nodemanager, logging to /home/hadoop/hadoop-2.4.1/logs/yarn-hadoop-nodemanager-localhost.out
```

### Etapa IV: Acessando o Hadoop no navegador

O número da porta padrão para acessar o Hadoop é 50070. Use o seguinte URL para obter os serviços do Hadoop em seu navegador.

```
http://localhost:50070/
```

![Navegador Hadoop](https://www.tutorialspoint.com/hive/images/hadoop_browser.jpg)

### Etapa V: verificar todos os aplicativos para cluster

O número da porta padrão para acessar todos os aplicativos do cluster é 8088. Use a seguinte url para visitar este serviço.

```
http://localhost:8088/
```

![Todos os aplicativos](https://www.tutorialspoint.com/hive/images/all_applications.jpg)

### Etapa 3: baixar o Hive

Usamos hive-0.14.0 neste tutorial. Você pode baixá-lo visitando o seguinte link [http://apache.petsads.us/hive/hive-0.14.0/. ](http://apache.petsads.us/hive/hive-0.14.0/)Vamos supor que ele seja baixado no diretório /Downloads. Aqui, baixamos o arquivo Hive chamado “apache-hive-0.14.0-bin.tar.gz” para este tutorial. O seguinte comando é usado para verificar o download:

```
$ cd Downloads
$ ls
```

No download bem-sucedido, você verá a seguinte resposta:

```
apache-hive-0.14.0-bin.tar.gz
```

## Etapa 4: Instalando o Hive

As etapas a seguir são necessárias para instalar o Hive em seu sistema. Vamos supor que o arquivo Hive seja baixado no diretório /Downloads.

### Extraindo e verificando o Hive Archive

O comando a seguir é usado para verificar o download e extrair o arquivo hive:

```
$ tar zxvf apache-hive-0.14.0-bin.tar.gz
$ ls
```

No download bem-sucedido, você verá a seguinte resposta:

```
apache-hive-0.14.0-bin apache-hive-0.14.0-bin.tar.gz
```

### Copiando arquivos para o diretório /usr/local/hive

Precisamos copiar os arquivos do superusuário “su -”. Os comandos a seguir são usados para copiar os arquivos do diretório extraído para o diretório /usr/local/hive”.

```
$ su -
passwd:

# cd /home/user/Download
# mv apache-hive-0.14.0-bin /usr/local/hive
# exit
```

### Configurando o ambiente para o Hive

Você pode configurar o ambiente Hive anexando as seguintes linhas ao arquivo **~/.bashrc** :

```
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=$CLASSPATH:/usr/local/Hadoop/lib/*:.
export CLASSPATH=$CLASSPATH:/usr/local/hive/lib/*:.
```

O comando a seguir é usado para executar o arquivo ~/.bashrc.

```
$ source ~/.bashrc
```

## Etapa 5: configurar o Hive

Para configurar o Hive com o Hadoop, você precisa editar o arquivo **hive-env.sh** , que é colocado no **diretório $HIVE_HOME/conf** . Os comandos a seguir redirecionam para a pasta de **configuração** do Hive e copiam o arquivo de modelo:

```
$ cd $HIVE_HOME/conf
$ cp hive-env.sh.template hive-env.sh
```

Edite o arquivo **hive-env.sh** anexando a seguinte linha:

```
export HADOOP_HOME=/usr/local/hadoop
```

A instalação do Hive foi concluída com sucesso. Agora você precisa de um servidor de banco de dados externo para configurar o Metastore. Usamos o banco de dados Apache Derby.

## Etapa 6: Baixar e instalar o Apache Derby

Siga as etapas abaixo para baixar e instalar o Apache Derby:

### Baixando Apache Derby

O comando a seguir é usado para fazer download do Apache Derby. Demora algum tempo para baixar.

```
$ cd ~
$ wget http://archive.apache.org/dist/db/derby/db-derby-10.4.2.0/db-derby-10.4.2.0-bin.tar.gz
```

O seguinte comando é usado para verificar o download:

```
$ ls
```

No download bem-sucedido, você verá a seguinte resposta:

```
db-derby-10.4.2.0-bin.tar.gz
```

### Extraindo e verificando o arquivo Derby

Os comandos a seguir são usados para extrair e verificar o arquivo do Derby:

```
$ tar zxvf db-derby-10.4.2.0-bin.tar.gz
$ ls
```

No download bem-sucedido, você verá a seguinte resposta:

```
db-derby-10.4.2.0-bin db-derby-10.4.2.0-bin.tar.gz
```

### Copiando arquivos para o diretório /usr/local/derby

Precisamos copiar do superusuário “su -”. Os comandos a seguir são usados para copiar os arquivos do diretório extraído para o diretório /usr/local/derby:

```
$ su -
passwd:
# cd /home/user
# mv db-derby-10.4.2.0-bin /usr/local/derby
# exit
```

### Configurando o ambiente para o Derby

Você pode configurar o ambiente Derby anexando as seguintes linhas ao arquivo **~/.bashrc** :

```
export DERBY_HOME=/usr/local/derby
export PATH=$PATH:$DERBY_HOME/bin
Apache Hive
18
export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar
```

O seguinte comando é usado para executar o arquivo **~/.bashrc** :

```
$ source ~/.bashrc
```

### Crie um diretório para armazenar o Metastore

Crie um diretório chamado data no diretório $DERBY_HOME para armazenar dados do Metastore.

```
$ mkdir $DERBY_HOME/data
```

A instalação do Derby e a configuração do ambiente agora estão concluídas.

## Etapa 7: configurando o metastore do Hive

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

## Etapa 8: Verificando a instalação do Hive

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

## Cursos úteis em vídeo