node {

     
        stage 'notify'
        notify('Started')
        
        try{

            def project_path = "spring-boot-samples/spring-boot-sample-atmosphere/"
            
            stage 'checkout'
            git 'https://github.com/vengolan/jenkins2-course-spring-petclinic.git'
            
            
            stage 'compile,test,package'
            sh label: '', script: "mvn clean verify"

                
        notify("Success")
    
    } catch (err) {
        notify("Error!!! ${err}")
        currentBuild.result = 'FAILURE'
    }
    
                
            stage 'archive'
            
            publishHTML([allowMissing: true, 
                         alwaysLinkToLastBuild: false, 
                         keepAll: true, 
                         reportDir: 'target/site/jacoco/', 
                         reportFiles: 'index.html', 
                         reportName: 'Code Coverage', 
                         reportTitles: 'Jacoco Report']
                         )
            
            step(
                    [
                    $class: 'JUnitResultArchiver', 
                    testResults: 'target/surefire-reports/TEST*xml'
                    ]
                )
            
            archiveArtifacts(
                 allowEmptyArchive: true, 
                 artifacts: 'target/*ar'
                 )
            


}

def notify(status){
    emailext (
            subject: "STATUS: ${status} - JOB: ${env.JOB_NAME} - [${env.BUILD_NUMBER}]", 
            to: 'venkat.r@greyamnp.com',
            body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}
