TheHive
=========

This role will install an instance of the tool "Cortex" on the selected server.
The role was designed to install "StandAlone" servers of "Cortex", it will not support installing "Cortex" on one server and the ELK DB on another server.

If you are installing a pair of "The Hive" and "Cortex" servers, first install the "Cortex" server to get the API Key for the connection between "The Hive" and "Cortex".

Requirements
------------

This role was tested and validated for:

 - CentOS7

Role Variables
--------------

This role comes with 6 sets of variables:

 1) Cortex Configuration variables:  
    These variables are used to define how your "Cortex" instance will be configured.  
    The variables are:  
    HOSTNAME: choose a hostname for your server  
    FQDN: the Fully Qualified Domaine Name of your server  
    CORTEX_IP: the IP address used by the "Cortex" server  
    CORTEX_BASEURL: the base URL you will use to connect to the "Cortex" web interface  
    ORG_ID: The ID or name of your Organisation. Will be used in "Cortex" to configure the organisation profile.  
    CORTEX_EMAIL: The email that the "Cortex" instance will impersonate when sending email notifications.  
    CORTEX_REPO_FILE: The location of the "Cortex yum repository file (default: "/etc/yum.repos.d/thehive-project.repo")
    CORTEX_CONF_FILE: The location of the "Cortex" configuration file (default: /etc/cortex/application.conf)
    CORTEX_AR_REPO: The location of the Github repository of the Cortex Analyzers and Responders (default: "https://github.com/TheHive-Project/Cortex-Analyzers")
    CORTEX_AR_DIRECTORY: The location where to clone the CORTEX_AR_REPO (default: "/opt/Cortex-Analyzers")
    CORTEX_A_DIRECTORY: The location of the analyzers folder on the system (default: "/opt/Cortex-Analyzers/analyzers")
    CORTEX_R_DIRECTORY: The location of the responders folder on the system (default "/opt/Cortex-Analyzers/responders")
    CORTEX_VIRTUALENV: The location of the Python Virtual Environment folder (default: "/opt/VECortex")

 2) Advanced Cortex Settings variables:
    These variables are for finetuning the "Cortex" server installation.
    /!\ Do not modify unless you are certain of what you are doing. /!\
    CORTEX_USE_ADVANCED_SETTINGS: Flag to indicate if you will use the advanced settings paramaters (true or false)
    CORTEX_LONGPOL_REFRESH: The longpolling refresh rate
    CORTEX_LONGPOL_CACHE: The lonpolling cache timer
    CORTEX_LONGPOL_NEXT_ITEM_WAIT: The interval between two longpol requests
    CORTEX_LONGPOL_GLOBAL_MAX_WAIT: The maximum amount of time between two longpol requests
    CORTEX_CACHE_JOB: The duration for which a Cortex job (analyser or responder) is cached
    CORTEX_CACHE_USER: 
    CORTEX_CACHE_ORG: 
    CORTEX_MAX_MEM_BUFFER: The maximum buffer size in memory.
    CORTEX_MAX_DISK_BUFFER: The maximum buffer size on disk.
    
 3) ElasticSearch configuration variables  
    These variables are used to define how the ElasticSearch DB engine used by "Cortex" will be configured.  
    The variables are:  
    ELK_REPO_FILE: The location of the yum repository file to be created to add the ELK repo to your yum configuration  
    ELK_CONF_FILE: The location of the ELK configuration file.  
    ELK_HOST: the IP address or URL where the ELK server is located. (By default this should be 127.0.0.1)  
    ELK_CLUSTER_NAME: The name you want to give to the ELK cluster  
    ELK_NODE_NAME: The name you want to give to the ELK node (You can have multiple nodes per clusters. This Ansible will not support multiple node setup)  
    ELK_INDEX: The name of your ELK index (Where all your "Cortex" entries will be stored)  

 4) SSL Configuration
    These variables define if you will make the "Cortex" instance run over SSL and with a Self-Signed or Imported certificate.
    SSL: Whether to use SSL or not (true or false)
    SSL_SELF_SIGNED: Whether to use a self signed certificate or import one from the ansible vault (true or false)

 5) OpenSSL settings for cert  
    These variables are used to configure and generate the self signed certificates used by "The Hive"  
    The variables are:  
    OPENSSL_C: The country code  
    OPENSSL_ST: The area name  
    OPENSSL_L: The city name  
    OPENSSL_O: The organisation ID - This variable is by default populated by the ORG_ID variable  
    OPENSSL_CN: The common name - This variable is by default populated by the FQDN variable  
    CORTEX_KEY: The certificate private key file name used by "Cortex"  
    CORTEX_CSR: The certificate signing request file name used by "Cortex"  
    CORTEX_CRT: The certificate file name used by "Cortex"
    CORTEX_CA: The certificate authority cert file name used by "Cortex"

 6) Auth settings variables:
    This variable will define which type of authentication mechanism will be used on the "Cortex" instance.
    AUTH_PROVIDER: Set this variable to define which authentication provider you want to use. Currently only local, ad, and ldap are supported. /!\ WARNING /!\ If you remove local, you cannot login with the local admin anymore! 

 7) LDAP Config  
    These variables are used to configure the LDAP connection for the "Cortex" user authentication  
    LDAP_SERVER_NAMES: Defines the server to contact to perform the LDAP authentication challenge  
    LDAP_BIND_DN: The binding string to use to connect to your LDAP  
    LDAP_BASE_DN: The base DN to use to filter which users are allowed to connect to "Cortex" via the LDAP connection  
    LDAP_FILTER: The LDAP item to check the login user name against when performing the authentication challenge against LDAP.  

 8) Nginx configuration variables  
    This variable is used to define the file path of the Nginx conf file.  

 9) Ansible Vault Variables
    These variables are stored in the ansible vault as they are sensitive and should not be stored in cleartext on your ansible server.
    You can create a vault by using the command: ansible-vault create /path/to/vault/file
    cortex_http_secret: the secret string to use for the hive http sessions
    ldap_bind_pw: the password for the bind user when using LDAP connection

Dependencies
------------

None

Example Playbook
----------------

Example Playbook

    - name: CORTEX 
      hosts: cortex001
      roles:
        - ansible-CentOS-Cortex

    # ansible vault to use
      vars_files:
        - /etc/ansible/vaults/cortex_vault

License
-------

EUPL v1.2

Author Information
------------------

Bertrand Varlet
