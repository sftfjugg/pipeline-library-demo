// 顶级node意味着脚本化管道,顶级pipeline意味着声明性管道.
//开发环境
def DEPLOY_DEV_HOST = [ '10.10.1.2','10.10.1.3','10.10.1.4']
//测试环境
def DEPLOY_TEST_THOST = [ '10.10.1.12','10.10.1.13','10.10.1.14']
//生产环境
def DEPLOY_PRO_THOST = [ '10.10.1.22','10.10.1.23','10.10.1.24']

def project = "agent"
def app_name = "agent"
def git_address = "ssh://git@git.example.com:10022/cloud3d/${project}.git"

// 认证
def SSH_CREDENTIAL = "/root/.ssh/"
def SSH_KEY = "/home/jenkins/.ssh/"

node() {
    // 拉去go环境镜像
    agent {
        docker { 
            label 'jenkins-dev'
            image 'golang:1.17-alpine' 
            args "-v ${SSH_KEY}:${SSH_CREDENTIAL}"
        }
    }

    parameters {
    //string类型参数
    string(name: 'youvar', defaultValue: 'sichenhao', description: '给我一个你的参数')
    //choice的默认值，为第一个参数选项
    choice (name: 'deploymode',choices: ['deploy', 'rollback'],description: '选择部署方式', )
    //git参数
    gitParameter( name: 'branch',branchFilter: 'origin/(.*)',defaultValue: 'dev', type: 'PT_BRANCH_TAG',description: '选择git分支')
    }

    environment { code_check_status = "" }

    stages {
        stage ("git_clone") {
            when {
                environment name:'deploymode', value:'deploy' 
            }           
            steps {  
             git branch: "$branch",  url: "$git_address"
            }
        }

        stage ("build") {
            when {
                environment name:'deploymode', value:'deploy' 
            }    
            steps {  
             println("开始构建${branch}分支")
            }
        }

        stage ("docker_build") {
            when {
                environment name:'deploymode', value:'deploy' 
            }    
            steps {  
                println("开始制造镜像")
            //获取git当前head简短
                script{
                build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                println(build_tag)
                }
            }
        }

        stage ("deploy") {
            when {
                environment name:'deploymode', value:'deploy' 
            }
            steps {  
                script {
                    switch("${branch}"){
                        case 'dev':
                            println("开始部署${branch}分支")
                            for (deployip in DEPLOY_DEV_HOST){
                                println("${deployip}")
                            }
                        break;
                        case 'test':
                            println("开始部署${branch}分支")
                            for (deployip in DEPLOY_TEST_THOST){
                                println("${deployip}")
                            }
                            break;
                        case 'master':
                            println("开始部署${branch}分支")
                            for (deployip in DEPLOY_PRO_THOST){
                                println("${deployip}")
                            }
                        break;
                    }   
                }              
            }
        }
 
        stage ("rollback") {
            when {
                environment name:'deploymode', value:'rollback' 
            }
            steps {  
            println("开始回滚")
                script {
                    switch("${branch}"){
                        case 'dev':
                            println("开始回滚${branch}环境")
                            for (rollbackip in DEPLOY_PRO_THOST){
                                println("${rollbackip}")
                            }
                        break;
                        case 'test':
                            println("开始回滚${branch}环境")
                            for (rollbackip in DEPLOY_PRO_THOST){
                                println("${rollbackip}")
                            }
                            break;
                        case 'master':
                            println("开始回滚${branch}环境")
                            for (rollbackip in DEPLOY_PRO_THOST){
                                println("${rollbackip}")
                            }
                        break;
                    }   
                }
            }
        }
    }
}
