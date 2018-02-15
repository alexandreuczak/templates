node {
    parameters {
        string(name: 'LOJAS', defaultValue: '', description: 'Lojas que receberão o artefato')
    }
    stage('Enviando Artefato'){
        sshagent (credentials: ['dcrpm']) {
            echo 'Enviando via SSH'
            sh 'scp -o StrictHostKeyChecking=no -l 400 /tmp/sam*.rpm root@172.17.0.3:/tmp/sam.rpm'
        
            def lojas = params.LOJAS.split()
            
            for (String loja : lojas) {
                def script = 'echo $(printf "L%03d" ' + loja + ')'
                def formatado = sh (returnStdout: true, script: "${script}")
                //def caminhoLoja = "$formatado".replace("\n", "") + ":/u/pdv/linux/PDV/"
                def caminhoLoja = "$formatado".replace("\n", "")
                print "${caminhoLoja}"
                sh 'ssh root@172.17.0.3 mkdir -p /tmp/' + caminhoLoja + '/pdv'
                sh 'ssh root@172.17.0.3 cp -rf /tmp/sam.rpm /tmp/' + caminhoLoja + '/pdv/sam.rpm'
            }
        }
        //print "${params.LOJAS}"
    }
}

