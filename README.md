// create environment for java and maven so it can access any where

JAVA_HOME=/usr/lib/jvm/java-21-amazon-corretto.x86_64
M2_HOME=/opt/maven
M2=/opt/maven/bin
PATH=$PATH:$HOME:/bin/$JAVA_HOME/bin:$M2:$M2_HOME
export PATH



