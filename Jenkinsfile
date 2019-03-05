node {
    
    currentBuild.displayName = "BUILD# $build_ID-$Name"
    stage('DATA ACQUISITION'){
        
        sshagent(['fsf_id_rsa']) {
            sh "ssh -o StrictHostKeyChecking=no root@192.168.17.129"
            sh "ssh root@192.168.17.129 chmod +x /usr/local/freesurfer/subjects/jenkins_multiple-subjects.sh"
            sh "ssh root@192.168.17.129 /usr/local/freesurfer/subjects/jenkins_multiple-subjects.sh list $Name $Dataset"
            sh "scp "
        }
    }
    
    stage('DATA PROCESSING (Freesurfer)'){
        sshagent(['fsf_id_rsa']) {
            File subjectFile = new File('$JENKINS_HOME/jobs/JOB_NAME/builds/subjectsList.txt')
            def subjects = subjectFile.readLines()    
            for (subject in subjects) {
                def currentSubject = "${subject}" 
                build job: 'jb_recon-all_simple', parameters: [string(name: 'Subject_dir', value: "${Dataset}"), string(name: 'Subject', value: "${currentSubject}")], quietPeriod: 1, wait: false
            }
        }
    }
    
    stage('NOTIFICATION AND REPORT'){
        echo "NOTIFICATION AND REPORT"
    }
}
