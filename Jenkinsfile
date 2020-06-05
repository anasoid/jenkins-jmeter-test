// While you can't use Groovy's .collect or similar methods currently, you can
// still transform a list into a set of actual build steps to be executed in
// parallel.

// Our initial list of strings we want to echo in parallel
def stringsToEcho = ["192.168.99.100:2222", "192.168.99.100:2223", "192.168.99.100:2224"]

// The map we'll store the parallel steps in before executing them.
def stepsForParallel = stringsToEcho.collectEntries {
    ["echoing ${it}" : transformIntoStep(it)]
}

// Actually run the steps in parallel - parallel takes a map as an argument,
// hence the above.
parallel stepsForParallel



// Take the string and echo it.
def prepareJmeterNode(serverSSH) {
    // We need to wrap what we return in a Groovy closure, or else it's invoked
    // when this method is called, not when we pass it to parallel.
    // To do this, you need to wrap the code below in { }, and either return
    // that explicitly, or use { -> } syntax.
    return {
        stage(serverSSH) {
            echo serverSSH
        }
        


    stage('Prepare Node  ' + serverSSH) {
      def remote = [:]
          remote.name = serverSSH
      remote.host = server
      remote.user = 'root'
      remote.password = 'root'
      remote.allowAnyHosts = true
      sshCommand remote: remote, command: "ls -lrt"
      sshCommand remote: remote, command: "for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done"
    }
    }
}