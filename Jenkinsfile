properties([parameters([[$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Region', randomName: 'choice-parameter-10001463150357', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''import groovy.json.JsonSlurper
import groovy.json.JsonBuilder
import groovy.json.JsonOutput

def regions = "aws ec2 describe-regions --query Regions --region us-east-1"
def proc = regions.execute() 
proc.waitFor()

def output = proc.in.text
def exitcode= proc.exitValue()
def error = proc.err.text
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
''']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Cluster', randomName: 'choice-parameter-10001464974566', referencedParameters: 'Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''def command = "aws ecs list-clusters --query clusterArns --region ${Region} --output text"


def proc = command.execute()
proc.waitFor()

def output = proc.in.text
def exitcode= proc.exitValue()
def error = proc.err.text

if (error) {
	println "Std Err: ${error}"
	println "Process exit code: ${exitcode}"
	return exitcode
}

def arrays = []
for(i in output.tokenize()){
   arrays.add(i.split(\'/\')[1])
}

return arrays''']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Service', randomName: 'choice-parameter-10001467112883', referencedParameters: 'Cluster,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''service = service(Cluster,Region)

def service(name,region) {
    command = "aws ecs list-services --cluster ${name} --query serviceArns --region ${region} --output text"


    def proc = command.execute() 
    proc.waitFor()

    def output = proc.in.text
    def exitcode= proc.exitValue()
    def error = proc.err.text

    if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }

    println output
  def arrays = []
  for(i in output.tokenize()){
    arrays.add(i.split(\'/\')[2])
  }
    return arrays
}''']]], [$class: 'DynamicReferenceParameter', choiceType: 'ET_UNORDERED_LIST', name: 'Current task version', omitValueField: false, randomName: 'choice-parameter-10001469218864', referencedParameters: 'Cluster,Service,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''  def taskname = "aws ecs describe-services --cluster ${Cluster} --services ${Service} --query services[*].taskDefinition --region ${Region} --output text"

def taskproc = taskname.execute()
taskproc.waitFor()

    def outputnew = taskproc.in.text
def exitcodenew = taskproc.exitValue()
    def errornew = taskproc.err.text

    if (errornew) {
    	println "Std Err: ${errornew}"
    	println "Process exit code: ${exitcodenew}"
    	return exitcodenew
    }

def array = []

array.add(outputnew.split(\'/\')[1])
return array''']]], [$class: 'DynamicReferenceParameter', choiceType: 'ET_UNORDERED_LIST', name: 'Image/s in current task definition', omitValueField: false, randomName: 'choice-parameter-26359019482233', referencedParameters: 'Cluster,Service,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''import groovy.json.JsonSlurper
import groovy.json.JsonBuilder
import groovy.json.JsonOutput

def taskname = "aws ecs describe-services --cluster ${Cluster} --services ${Service} --query services[*].taskDefinition --region ${Region} --output text"

def taskproc = taskname.execute()
taskproc.waitFor()

    def outputnew = taskproc.in.text
    def exitcodenew = taskproc.exitValue()
    def errornew = taskproc.err.text

    if (errornew) {
    	println "Std Err: ${errornew}"
    	println "Process exit code: ${exitcodenew}"
    	return exitcodenew
    }
//a = outputnew.split(\'/\')[1]

def array = []

abcde = outputnew.split(\'/\')[1]

def images = "aws ecs describe-task-definition --task-definition ${abcde} --query taskDefinition.containerDefinitions"
def proc = images.execute() 
proc.waitFor()

def output = proc.in.text
def exitcode= proc.exitValue()
def error = proc.err.text
def newf = ""

if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }

//println(output.tokenize())


for(i in output.tokenize()){
  newf += i
}

def jsonSlurper = new JsonSlurper()
def parseJson = jsonSlurper.parseText(newf)

//println(newf)
//println(parseJson[1])
def arrays = []
for(i in 0..(parseJson.size()-1)) {
arrays.add(parseJson[i].image)
}
return arrays''']]], [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT', filterLength: 1, filterable: true, name: 'Task', randomName: 'choice-parameter-10001470922664', referencedParameters: 'Cluster,Service,Region', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: ''], script: [classpath: [], sandbox: false, script: '''task = taskname(Cluster,Service,Region)

def taskname(cluster,service,region){
    def taskname = "aws ecs describe-services --cluster ${cluster} --services ${service} --query services[*].taskDefinition --region ${region} --output text"

def taskproc = taskname.execute()
taskproc.waitFor()

    def outputnew = taskproc.in.text
def exitcodenew = taskproc.exitValue()
    def errornew = taskproc.err.text

    if (errornew) {
    	println "Std Err: ${errornew}"
    	println "Process exit code: ${exitcodenew}"
    	return exitcodenew
    }
a = outputnew.split(\'/\')[1].split(\':\')[0]

def command = "aws ecs list-task-definitions --family-prefix ${a} --query taskDefinitionArns --region ${region} --output text"


    def proc = command.execute() 
    proc.waitFor()

    def output = proc.in.text
def exitcode= proc.exitValue()
    def error = proc.err.text

    if (error) {
    	println "Std Err: ${error}"
    	println "Process exit code: ${exitcode}"
    	return exitcode
    }
def arrays = []
  for(i in output.tokenize()){
    arrays.add(i.split(\'/\')[1])
  }
    return arrays
    
}''']]], choice(choices: ['true', 'false'], name: 'force_new_deployment')])])

pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }


    stages {
        stage('testing') {
            steps {
                println "Cluster Name: ${Cluster}"
                println "Service Name: ${service}"
                
                println "Task: ${Task}"
                println "Force: ${force_new_deployment}"
            sh 'aws ecs list-clusters --region us-east-1'  
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
