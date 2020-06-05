

// While you can't use Groovy's .collect or similar methods currently, you can
// still transform a list into a set of actual build steps to be executed in
// parallel.

// Our initial list of strings we want to echo in parallel
def master = "192.168.99.100:2222"
def slaves = ["192.168.99.100:2223", "192.168.99.100:2224"]

node {
checkout scm 

// The map we'll store the parallel steps in before executing them.
def prepareForParallel = slaves.collectEntries {
    ["echoing ${it}" : prepareJmeterNode(it)]
}
prepareForParallel.putAt("echoing ${master}",prepareJmeterNode(master))


// The map we'll store the parallel steps in before executing them.
def slaveForParallel = slaves.collectEntries {
    ["echoing ${it}" : prepareJmeterNode(it)]
}
prepareForParallel.putAt("echoing ${master}",prepareJmeterNode(master))

// Actually run the steps in parallel - parallel takes a map as an argument,
// hence the above.
parallel prepareForParallel

startJmeterMaster(master)

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
      sshRemove remote: remote, path: "/test"
      sshRemove remote: remote, path: "/reports"
      shCommand remote: remote, command: "mkdir /reports"
      sshPut remote: remote, from: 'test', into: '/'
      sshCommand remote: remote, command: "ls -la /test"

    }
  }
  
}
// PrepareJmeterNode
def startJmeterMaster(serverSSH) {

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
      sshCommand remote: remote, command: "cd /test; jmeter -X -n -f -r -e -l /tmp/results.jtl  -t  ./example.jmx -o /reports "


    }
  }
  
}