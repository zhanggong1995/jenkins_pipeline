list_huidu1=[]
list_huidu2=[]
def ssh_command(hostname,host,cred_id,cmd){
	def remote = [:]
	remote.name = hostname
	remote.host = host
	remote.allowAnyHosts= true
	withCredentials([usernamePassword(credentialsId: cred_id, passwordVariable: 'password', usernameVariable: 'userName')]) {
		remote.user = userName
		remote.password = password
	}
	sshCommand remote: remote, command: cmd
}
node { 
	stage('选角色'){
		dataMap=input message: '选择角色',
		parameters: [
		    booleanParam(defaultValue: false, description: '', name: 'doc-batch'),
			booleanParam(defaultValue: false, description: '', name: 'erp-batch'),
			booleanParam(defaultValue: false, description: '', name: 'erp-doc'),
			booleanParam(defaultValue: false, description: '', name: '1erp-huidu'),
			booleanParam(defaultValue: false, description: '', name: '1erp-service'),
			booleanParam(defaultValue: false, description: '', name: 'erp-trade-service'),
			booleanParam(defaultValue: false, description: '', name: 'erp-sys-serv'),
			booleanParam(defaultValue: false, description: '', name: '2erp-huidu'),
			booleanParam(defaultValue: false, description: '', name: '2erp-service'),
			booleanParam(defaultValue: false, description: '', name: 'erp-service-es'),
			booleanParam(defaultValue: false, description: '', name: 'erp-online')
		    ]
	}
	stage('构造并检查角色'){
		if (dataMap['doc-batch'] == true )			{ list_huidu1+="doc-batch" }
		if (dataMap['erp-batch'] == true )			{ list_huidu1+="erp-batch" }
		if (dataMap['erp-doc'] == true )			{ list_huidu1+="erp-doc" }
		if (dataMap['1erp-huidu'] == true )			{ list_huidu1+="1erp-huidu" }
		if (dataMap['1erp-service'] == true )		{ list_huidu1+="1erp-service" }
		if (dataMap['erp-trade-service'] == true )	{ list_huidu1+="erp-trade-service" }
		if (dataMap['erp-sys-serv'] == true )		{ list_huidu1+="erp-sys-serv" }
		
		if (dataMap['2erp-huidu'] == true )			{ list_huidu2+="2erp-huidu" }
		if (dataMap['2erp-service'] == true )		{ list_huidu2+="2erp-service" }
		if (dataMap['erp-service-es'] == true )		{ list_huidu2+="erp-service-es" }
		if (dataMap['erp-online'] == true )			{ list_huidu2+="erp-online" }
		print list_huidu1
		print list_huidu2
	}
	stage('执行拉包'){

		parallel '灰度1的拉包':{
			if (list_huidu1!=[]){
					cmd1=''
					for (i in list_huidu1){
						cmd1+=(i+',')
					}
					ssh_command('test','172.16.4.4','83726ffe-6d9f-4964-8d26-88f6b0de7083','python /root/test.py')
					print cmd1
				}
		}, 	'灰度2的拉包':{
			if (list_huidu2!=[]){
				cmd2=''
					for (i in list_huidu2){
						cmd2+=i
						cmd2+=','
					}
					ssh_command('test','172.16.4.4','83726ffe-6d9f-4964-8d26-88f6b0de7083','python /root/test.py')
					print cmd2
				}
		},failFast: false
	}
}
