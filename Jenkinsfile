properties([parameters([[$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Region', randomName: 'choice-parameter-10001463150357', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''import groovy.json.JsonSlurper

def regions = "aws ec2 describe-regions --query Regions --region us-east-1"
def region = regions.execute() 
region.waitFor()

def output = region.in.text
def exitcode= region.exitValue()
def error = region.err.text
def refString = ""


if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }

for(i in output.tokenize()){
  refString += i
}

def jsonSlurper = new JsonSlurper()
def parseJson = jsonSlurper.parseText(refString)

def arrays = []
for(i in 0..(parseJson.size()-1)) {
arrays.add(parseJson[i].RegionName)
}
return arrays
''']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Cluster', randomName: 'choice-parameter-10001464974566', referencedParameters: 'Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''def cluster = "aws ecs list-clusters --query clusterArns --region ${Region} --output text"

def clusterExec = cluster.execute()
clusterExec.waitFor()

def output = clusterExec.in.text
def exitcode= clusterExec.exitValue()
def error = clusterExec.err.text

if (error) {
	println "Std Err: ${error}"
	println "Process exit code: ${exitcode}"
	return exitcode
}

def arrays = []
for(i in output.tokenize()){
   arrays.add(i.split(\'/\')[1])
}

return arrays''']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Service', randomName: 'choice-parameter-10001467112883', referencedParameters: 'Cluster,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''service = "aws ecs list-services --cluster ${Cluster} --query serviceArns --region ${Region} --output text"


    def serviceExec = service.execute() 
    serviceExec.waitFor()

    def output = serviceExec.in.text
    def exitcode= serviceExec.exitValue()
    def error = serviceExec.err.text

    if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }

  def arrays = []
  for(i in output.tokenize()){
    arrays.add(i.split(\'/\')[2])
  }
    return arrays''']]], [$class: 'DynamicReferenceParameter', choiceType: 'ET_UNORDERED_LIST', name: 'Current task version', omitValueField: false, randomName: 'choice-parameter-10001469218864', referencedParameters: 'Cluster,Service,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''def taskname = "aws ecs describe-services --cluster ${Cluster} --services ${Service} --query services[*].taskDefinition --region ${Region} --output text"

def taskExec = taskname.execute()
taskExec.waitFor()

    def output = taskExec.in.text
    def exitcode = taskExec.exitValue()
    def error = taskExec.err.text

    if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }

def array = []

array.add(output.split(\'/\')[1])
return array''']]], [$class: 'DynamicReferenceParameter', choiceType: 'ET_UNORDERED_LIST', name: 'Image/s in current task definition', omitValueField: false, randomName: 'choice-parameter-26359019482233', referencedParameters: 'Cluster,Service,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''import groovy.json.JsonSlurper


def taskname = "aws ecs describe-services --cluster ${Cluster} --services ${Service} --query services[*].taskDefinition --region ${Region} --output text"

def taskExec = taskname.execute()
taskExec.waitFor()

    def output = taskExec.in.text
    def exitcode = taskExec.exitValue()
    def error = taskExec.err.text

    if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }

def array = []

taskDef = output.split(\'/\')[1]

def images = "aws ecs describe-task-definition --task-definition ${taskDef} --region ${Region} --query taskDefinition.containerDefinitions"
def imageExec = images.execute() 
imageExec.waitFor()

def imageExecOutput = imageExec.in.text
def imageExecExitcode = imageExec.exitValue()
def imageExecError = imageExec.err.text
def refString = ""

if (imageExecError) {
    	println "Std Err: ${imageExecError}"
    	println "Process exit code: ${imageExecExitcode}"
    	return imageExecExitcode
    }


for(i in imageExecOutput.tokenize()){
  refString += i
}

def jsonSlurper = new JsonSlurper()
def parseJson = jsonSlurper.parseText(refString)

def arrays = []
for(i in 0..(parseJson.size()-1)) {
arrays.add(parseJson[i].image)
}
return arrays''']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Task', randomName: 'choice-parameter-10001470922664', referencedParameters: 'Cluster,Service,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''def taskname = "aws ecs describe-services --cluster ${Cluster} --services ${Service} --query services[*].taskDefinition --region ${Region} --output text"
def taskproc = taskname.execute()
taskproc.waitFor()

def output = taskproc.in.text
def exitcode = taskproc.exitValue()
def error = taskproc.err.text

if (error) {
	println "Std Err: ${error}"
	println "Process exit code: ${exitcode}"
	return exitcode
}
family = output.split(\'/\')[1].split(\':\')[0]

def taskDef = "aws ecs list-task-definitions --family-prefix ${family} --query taskDefinitionArns --region ${Region} --output text"


def taskExec = taskDef.execute() 
taskExec.waitFor()
def taskExecOutput = taskExec.in.text
def taskExecExitcode= taskExec.exitValue()
def taskExecError = taskExec.err.text

if (taskExecError) {
	println "Std Err: ${taskExecError}"
	println "Process exit code: ${taskExecExitcode}"
	return taskExecExitcode
}
def arrays = []
  for(i in taskExecOutput.tokenize()){
    arrays.add(i.split('/')[1])
  }
return arrays''']]], choice(choices: ['true', 'false'], name: 'force_new_deployment')])])

pipeline {
    agent any


    stages {
        stage('testing') {
            steps {
                println "Cluster Name: ${Cluster}"
                println "Service Name: ${service}"
                
                println "Task: ${Task}"
                println "Force: ${force_new_deployment}"
            }  
        }
        
        stage ('Force deployment') {
      when {
                expression { force_new_deployment == 'true'}
            }
            steps {
                sh 'aws ecs update-service --cluster ${Cluster} --service ${Service} --task-definition ${Task} --force-new-deployment'
            }
    }
    
    stage ('Normal deployment') {
      when {
                expression { force_new_deployment == 'false'}
            }
            steps {
                sh 'aws ecs update-service --cluster ${Cluster} --service ${Service} --task-definition ${Task}'
            }
        }
    }
}
