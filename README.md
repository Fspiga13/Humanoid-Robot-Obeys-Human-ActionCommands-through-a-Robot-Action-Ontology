# Humanoid Robot Obeys Human Action Commands through a Robot Action Ontology
This software engine allows the Zora humanoid robot to execute natural language commands spoken by the user. To provide the robot with knowledge, we have defined an action robot ontology. The ontology is fed to a NLP engine that performs a machine reading of the input text (in natural language) given by a user and tries to identify action commands for the robot to execute. The system can work in two modes: SEQUENTIAL and HOLD. In SEQUENTIAL mode, each human expression correctly interpreted by the robot as an action command is performed by Zora which returns in its default posture afterwards. When in HOLD mode, the robot has knowledge of its current posture and performs the command only if it is compatible with its current state. In this mode, the robot does not return to its default posture. For example, if the user had told the robot to stand on its right leg in a first command, the robot cannot perform a following command stating to stand on its left leg as the two actions (raise left leg and raise right leg are incompatible). For each action that the robot can perform we modeled a corresponding element in the ontology that also includes a list of associated compatible and non-compatible actions. Our system also handles compound expressions (e.g. move your arms up) and multiple expressions (different commands within one sentence) that the robot understands and performs. A video showing the proposed system and the knowledge that is possible to extract from the human interaction with Zora is available at: https://youtu.be/CC9NzlbF0gQ.

## Getting started
### Prerequisites
First of all, you need to download or clone the repository. The necessary files to run whole application are the ZoraNlpReasoner folder and the ZoraActions.crg project.

Before to use the application, you need to install [Java](https://www.oracle.com/technetwork/java/javase/downloads/index.html) and [Python version 2.7](https://www.python.org/downloads/release/python-2710/).
You need to also download the following packages:
```
pip install requests
pip install hdfs
pip install bs4
pip install rdflib
pip install nltk
pip install graphviz
pip install stanfordcorenlp
```
Download [Stanford coreNLP](https://stanfordnlp.github.io/CoreNLP/) and move Stanford main folder into the TextToRDF folder.

Download and install Choregraphe. If you have troubles in the following steps using Python in Choregraphe use [this](http://doc.aldebaran.com/2-1/dev/python/install_guide.html).

### Installing

#### Step #1
To start Stanford coreNLP locally and listening on port 9000 run the following command after moving to the main Stanford folder:
(Remember that using Stanford locally requires about 4GB of free RAM)
```
cd /ZoraNlpReasoner/TextToRDF/stanford-corenlp-full-2018-10-05/
java --add-modules java.se.ee -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -port 9000 -timeout 15000
```

#### Step #2
After running Stanford, you can test the operation of the RDF creation tool from the Stanford output. 
Run the following command after moving to the src folder, TextToRDF subfolder:
```
cd /ZoraNlpReasoner/TextToRDF/src/
python xproject.py -t "<sentence>"
```
An example of an input sentence is: Zora, raise your right arm. This Step aims to verify that our tool TextToRDF returns the RDF of the input sentence.

#### Step #3
Passed the previous step, you can run the ZoraNlpReasoner main project, after moving to the ZoraNlpReasoner folder, with the command:
```
cd /ZoraNlpReasoner/
java -jar ZoraNlpReasoner.jar
```
to test that it communicates correctly with the TextToRDF tool. When the prject runs, shows the instruction for use and waits for an input command for the robot (*e.g. Zora, raise your right arm*).

Through the use of the project it is possible to test the behavior of the robot based on the sentences chosen as if they were given as input to the robot itself. Shows all the features without the need for *Choregraphe*.

#### Step #4
Once this phase is over, you can test the **complete application**, also using the Choregraphe software by importing the project from the ZoraActions.crg file. 
The *Reasoner NLP box*, located in the *Choregraphe* project, allows you to specify an URL, is set with the address: 
```
http://localhost:5003
```
this setting is correct when you want to use the **simulated robot**. 
To use the Reasoner with *Choregraphe*, run the ZoraNlpReasoner.jar with the command:
```
java -jar ZoraNlpReasoner.jar use zora
```
#### Step #5
By starting the main project and the *Choregraphe* software it is possible to use the **console**, present in *Choregraphe*, to give the input sentences to the robot and see its behavior in the **simulated robot**.

To use the application with the **real robot** it is necessary to connect both the Zora robot and the computer to an external hotspot. You need to install [ngrok](https://ngrok.com/download) and start it with the command (from the directory where it is installed):
```
./ngrok http 5003
```
to generate the new URL to replace *localhost:5003* in the *Reasoner NLP box*. Ngrok should be started **every time** a new connection is made as the generated URL is disposable, with a limited duration.

### Using whole application
To **start** the application connect the Zora robot and the computer to an external hotspot. Then, start Stanford listening on *port 9000*, start ngrok on *port 5003* and replace the old URL, in the *Reasoner NLP box*, with the one provided by ngrok. Run the ZoraNlpReasoner main project with the command:
```
java -jar ZoraNlpReasoner.jar use zora
```
and the ZoraActions software with *Choregraphe*. It is possible to communicate with the **real robot** via voice commands or through the Choregraphe console.

To **stop** the application it is possible to give commands such as *stop* or *quit*. Then, the executions of ZoraNlpReasoner, ngrok and Stanford coreNLP must be manually stopped.

## Repository contents
### ZoraNlpReasoner
This folder contains the main project (ZoraNlpReasoner.jar), including the **TextToRDF tool** and the **Ontologies**. To use the application it is necessary to add Stanford coreNLP inside the TextToRDF folder. This section of the application contains the NLP Engine we have developed.

### ZoraActions.crg
This file contains the *Choregraphe* software for the Zora robot. This section of the application deals with the management  of the robot and the interaction with the NLP Engine from the robot. 

### IncompatibilityTable
The table shows for each action in the ontology the actions that cannot precede it and therefore are incompatible. Actions that have no incompatibility can always be performed.

## Authors
* **Federico Spiga** - *University of Cagliari* - [Fspiga13](https://github.com/Fspiga13)
