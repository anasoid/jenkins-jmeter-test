

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
def startMaster =   [ "Start ${master}": startJmeterMaster(master) ]


// Actually run the steps in parallel - parallel takes a map as an argument,
// hence the above.

    


parallel prepareForParallel

parallel  startMaster
 



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

    }
  }
  
}
// PrepareJmeterNode
def startJmeterMaster(serverSSH) {

    return {



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
      sshCommand remote: remote, command: "echo $PATH"
      sshCommand remote: remote, command: "printenv"
      sshCommand remote: remote, command: "cd /test; jmeter -X -n -f  -e -l /tmp/results.jtl  -t  ./example.jmx -o /reports "


    }
  }
  
}