

// While you can't use Groovy's .collect or similar methods currently, you can
// still transform a list into a set of actual build steps to be executed in
// parallel.

// Our initial list of strings we want to echo in parallel
def master = "192.168.99.100:2222"
def slaves = ["192.168.99.100:2223", "192.168.99.100:2224"]

node {
checkout scm 

// Prepare Jmeter node.
def prepareForParallel = slaves.collectEntries {
    ["Prepare ${it}" : prepareJmeterNode(it)]
}
prepareForParallel.putAt("Prepare ${master}",prepareJmeterNode(master))

// Prepare Jmeter node.
def startSlaves = slaves.collectEntries {
    ["Start slave ${it}" : startJmeterSlave(it)]
}



// Prepare Jmeter node.
def startMaster =   [ "Start Master ${master}": startJmeterMaster(master) ]

def startNodes= [:]


startNodes.putAll(startSlaves)
startNodes.putAll(startMaster)
// Actually run the steps in parallel - parallel takes a map as an argument,
// hence the above.

    


parallel prepareForParallel

parallel  startNodes
 



}

// PrepareJmeterNode
def prepareJmeterNode(serverSSH) {

    return {



    stage('Prepare Node  ' + serverSSH) {
      echo serverSSH
      def remote = [:]
      def hostport= serverSSH.tokenize(":")
      remote.name = serverSSH
      remote.host = hostport[0]
      remote.port = Integer.parseInt(hostport[1])
      remote.user = 'root'
      remote.password = 'root'
      remote.allowAnyHosts = true
      sh "ls -l"
      sshCommand remote: remote, command: "ls -la /"
      sshCommand remote: remote, command: "rm -rf /test"
      sshCommand remote: remote, command: "rm -rf /reports"
      sshCommand remote: remote, command: "mkdir /reports"
      sshPut remote: remote, from: 'test', into: '/'
      sshCommand remote: remote, command: "ls -la /test"
      sshCommand remote: remote,pty:true, command: "printenv"

    }
  }
  
}
// PrepareJmeterNode
def startJmeterSlave(serverSSH) {

    return {



    stage('Start slave ' + serverSSH) {
      echo serverSSH
      def remote = [:]
      def hostport= serverSSH.tokenize(":")
      remote.name = serverSSH
      remote.host = hostport[0]
      remote.port = Integer.parseInt(hostport[1])
      remote.user = 'root'
      remote.password = 'root'
      remote.allowAnyHosts = true
      sshCommand remote: remote,  command: "export JAVA_HOME=/usr/local/openjdk-8; cd /jmeter/bin; /jmeter/bin/jmeter-server "


    }
  }
  
}
// PrepareJmeterNode
def startJmeterMaster(serverSSH) {

    return {
    stage('Master wait slaves ') {
      sh "sleep 5"

    }


    stage('Start Master ' + serverSSH) {
      echo serverSSH
      def remote = [:]
      def hostport= serverSSH.tokenize(":")
      remote.name = serverSSH
      remote.host = hostport[0]
      remote.port = Integer.parseInt(hostport[1])
      remote.user = 'root'
      remote.password = 'root'
      remote.allowAnyHosts = true
      sshCommand remote: remote,  command: "export JAVA_HOME=/usr/local/openjdk-8; cd /jmeter/bin; /jmeter/bin/jmeter -X -n -f -r -e -l /tmp/results.jtl  -t  /test/example.jmx -o /reports -Jremote_hosts=jmeter-slave1,jmeter-slave2"


    }
  }
  
}