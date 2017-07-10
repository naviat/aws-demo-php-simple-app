pipeline {

stage 'Dev'
node ('docker-cloud') {
    checkout scm
}

stage 'QA'
parallel(longerTests: {
    runTests(30)
}, quickerTests: {
    runTests(20)
})

stage name: 'Staging', concurrency: 1
node ('docker-cloud') {
    deploy 'staging'
}

input message: "Does staging look good?"
try {
    checkpoint('Before production')
} catch (NoSuchMethodError _) {
    echo 'Checkpoint feature available in CloudBees Jenkins Enterprise.'
}

stage name: 'Production', concurrency: 1
node ('docker-cloud'){
    echo 'Production server looks to be alive'
    deploy 'production'
    echo "Deployed to production"
}


def runTests(duration) {
    node {
        sh "sleep ${duration}"
        }
    }
    
 }
