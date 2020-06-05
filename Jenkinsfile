

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
prepareForParallel.putAt("echoing ${master}",master)



// Actually run the steps in parallel - parallel takes a map as an argument,
// hence the above.
parallel prepareForParallel

}

// Take the string and echo it.
def prepareJmeterNode(serverSSH) {
    // We need to wrap what we return in a Groovy closure, or else it's invoked
    // when this method is called, not when we pass it to parallel.
    // To do this, you need to wrap the code below in { }, and either return
    // that explicitly, or use { -> } syntax.
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
      sshPut remote: remote, from: 'test', into: '/'
      sshCommand remote: remote, command: "ls -la /test"

    }
  }
  
}