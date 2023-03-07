//六种参数类型
pipeline {
  agent any
  parameters {
    choice(
      name: 'model',
      choices: ['m1', 'm2', 'm3'],
      description: '选择模块'
    )
 
    string(
        name: 'hostname',
        defaultValue: '192.168.1.11',
        description: '主机地址'
    )
 
    text(
        name: 'remark',
        defaultValue: 'name: ada \n \
                sex: woman \n \
                age: 30',
        description: '个人信息'
    )
 
    booleanParam(
        name: 'is_test',
        defaultValue: true,
        description: '是否需要测试'
    )
 
    password(
        name: 'password',
        defaultValue: 'ada',
        description: '密码'
    )
 
    file(
        name: "config_file",
        description: "选择配置文件"
    )
  }
 
  stages {
        stage('构建') {
            steps {
                echo "构建 stage: 建模块为 : ${params.model} ..."
            }
        }
        stage('测试'){
            steps {
                echo "测试 stage: 测试: ${params.is_test} ..."
            }
        }
        stage('部署') {
            steps {
                echo "部署 stage: 主机名 : ${params.hostname} ..."
                echo "部署 stage: 密码 : ${params.password} ..."
                echo "部署 stage: 个人信息 : ${params.remark} ..."
            }
        }
  }
}
