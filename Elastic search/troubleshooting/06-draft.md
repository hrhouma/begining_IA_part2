```bash
root@elasticSrv1:/home/eleve# history
    1  nano install_elastic_stack.sh
    2  cat install_elastic_stack.sh
    3  #!/bin/bash
    4  # Fonction pour créer un dossier de logs s'il n'existe pas
    5  create_log_directory() {     LOG_DIR="/var/log/elastic_setup";     if [ ! -d "$LOG_DIR" ]; then         sudo mkdir -p "$LOG_DIR";         sudo chmod 755 "$LOG_DIR";     fi; }
    6  # Étape 1 - Mise à jour des paquets
    7  echo "Mise à jour des paquets..."
    8  sudo apt update && sudo apt install -y
    9  # Étape 2 - Installation de Elasticsearch
   10  echo "Installation d'Elasticsearch..."
   11  sudo apt install -y elasticsearch
   12  # Configuration et activation d'Elasticsearch
   13  echo "Configuration d'Elasticsearch..."
   14  sudo systemctl enable elasticsearch
   15  sudo systemctl start elasticsearch
   16  # Étape 3 - Installation de Kibana
   17  echo "Installation de Kibana..."
   18  wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   19  echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
   20  sudo apt update
   21  sudo apt install -y kibana
   22  # Configuration de Kibana
   23  echo "Configuration de Kibana..."
   24  sudo tee /etc/kibana/kibana.yml > /dev/null <<EOL
   25  server.port: 5601
   26  server.host: "0.0.0.0"
   27  elasticsearch.hosts: ["http://localhost:9200"]
   28  EOL
   29  # Activer et démarrer Kibana
   30  echo "Activation et démarrage de Kibana..."
   31  sudo systemctl enable kibana
   32  sudo systemctl start kibana
   33  # Création du dossier de logs
   34  create_log_directory
   35  # Étape 4 - Génération et sauvegarde du jeton d'enrôlement pour Kibana
   36  echo "Génération et sauvegarde du jeton d'enrôlement pour Kibana..."
   37  ENROLLMENT_TOKEN=$(sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana)
   38  echo "$ENROLLMENT_TOKEN" | sudo tee "$LOG_DIR/kibana_enrollment_token.txt" > /dev/null
   39  # Étape 5 - Obtention et sauvegarde du code de vérification de Kibana
   40  echo "Obtention et sauvegarde du code de vérification de Kibana..."
   41  VERIFICATION_CODE=$(sudo /usr/share/kibana/bin/kibana-verification-code)
   42  echo "$VERIFICATION_CODE" | sudo tee "$LOG_DIR/kibana_verification_code.txt" > /dev/null
   43  # Étape 6 - Réinitialisation et sauvegarde du mot de passe Elasticsearch
   44  echo "Réinitialisation et sauvegarde du mot de passe pour l'utilisateur elastic..."
   45  NEW_PASSWORD=$(sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic -b)
   46  echo "$NEW_PASSWORD" | sudo tee "$LOG_DIR/elastic_password.txt" > /dev/null
   47  # Vérification du statut des services
   48  echo "Vérification des statuts de Kibana et Elasticsearch..."
   49  sudo systemctl status kibana
   50  sudo systemctl status elasticsearch
   51  echo "Installation et configuration terminées."
   52  echo "Les informations importantes ont été sauvegardées dans $LOG_DIR"
   53  sudo ./install_elastic_stack.sh
   54  sh install_elastic_stack.sh
   55  sudo apt update
   56  wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   57  echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
   58  sudo apt update
   59  sudo apt install kibana
   60  sudo systemctl status kibana
   61  sudo systemctl enable kibana
   62  sudo systemctl start kibana
   63  sudo systemctl status kibana
   64  sudo nano /etc/kibana/kibana.yml
   65  sudo systemctl stop kibana
   66  sudo systemctl enable kibana
   67  sudo systemctl start kibana
   68  sudo systemctl status elasticsearch
   69  sudo apt update
   70  sudo apt install elasticsearch
   71  sudo systemctl status elasticsearch
   72  sudo systemctl enable elasticsearch
   73  sudo systemctl start elasticsearch
   74  sudo systemctl status kibana
   75  sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
   76  systemctl status elasticsearch
   77  GET _cluster/health
   78  GET _nodes/stats
   79  POST liste_cours/_doc
   80  { "nom_professeur": "Jean Dupon", "sigle_cours": "ABC123" }
   81  sudo nano /etc/elasticsearch/elasticsearch.yml
   82  curl -XGET localhost:9200
   83  apt install curl
   84  curl -XGET localhost:9200
   85  systemctl status elasticsearch
   86  sudo systemctl start elasticsearch
   87  sudo nano /etc/elasticsearch/elasticsearch.yml
   88  sudo systemctl stop elasticsearch
   89  sudo systemctl start elasticsearch
   90  curl -XGET localhost:9200
   91  sudo nano /etc/elasticsearch/elasticsearch.yml
   92  sudo systemctl stop elasticsearch
   93  sudo systemctl start elasticsearch
   94  curl -XGET localhost:9200
   95  sudo systemctl enable elasticsearch
   96  curl -XGET localhost:9200
   97  sudo journalctl -u elasticsearch
   98  sudo nano /etc/elasticsearch/elasticsearch.yml
   99  sudo systemctl stop elasticsearch
  100  sudo systemctl start elasticsearch
  101  curl -XGET http://localhost:9200
  102  root@elasticSrv1:/home/eleve# ^C
  103  root@elasticSrv1:/home/eleve# sudo nano /etc/elasticsearch/elasticsearch.yml
  104  root@elasticSrv1:/home/eleve# sudo systemctl stop elasticsearch
  105  root@elasticSrv1:/home/eleve# sudo systemctl start elasticsearch
  106  root@elasticSrv1:/home/eleve#
  107  root@elasticSrv1:/home/eleve# curl -XGET http://localhost:9200
  108  curl: (52) Empty reply from server
  109  curl -XGET http://localhost:9200
  110  curl -k -X GET 'https://localhost:9200/'
  111  sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
  112  curl -u elastic:c+vdv5FUzys5hft5*8Fs -X GET 'https://localhost:9200/'
  113  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  114  root@elasticSrv1:/home/eleve# sudo nano /etc/elasticsearch/elasticsearch.yml
  115  nano /etc/elasticsearch/elasticsearch.yml
  116  sudo systemctl restart elasticsearch
  117  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  118  curl -k -X GET 'https://localhost:9200/'
  119  curl -X GET 'https://localhost:9200/'
  120  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  121  sudo systemctl status elasticsearch
  122  nano /etc/elasticsearch/elasticsearch.yml
  123  sudo systemctl restart elasticsearch
  124  sudo systemctl status elasticsearch
  125  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  126  grep "xpack.security.enabled:" /etc/elasticsearch/elasticsearch.yml
  127  grep -n "xpack.security.enabled:" /etc/elasticsearch/elasticsearch.yml
  128  nano /etc/elasticsearch/elasticsearch.yml
  129  sudo systemctl restart elasticsearch
  130  sudo systemctl status elasticsearch
  131  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  132  curl -X GET 'https://localhost:9200/'
  133  curl -k -X GET 'https://localhost:9200/'
  134  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  135  nano /etc/elasticsearch/elasticsearch.yml
  136  sudo systemctl restart elasticsearch
  137  sudo systemctl status elasticsearch
  138  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET 'https://localhost:9200/'
  139  nano /home/eleve/.curlrc
  140  chmod 600 /home/eleve/.curlrc
  141  curl -k -X GET 'https://localhost:9200/'
  142  nano configure_elastic.sh
  143  curl -k -X GET 'https://localhost:9200/'
  144  sudo systemctl restart elasticsearch
  145  curl -k -X GET 'https://localhost:9200/'
  146  history
  147  cat /root/.curlrc
  148  ls
  149  nano /root/.curlrc
  150  chmod 600 /root/.curlrc
  151  curl -k -X GET 'https://localhost:9200/'
  152  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X PUT "https://localhost:9200/news"
  153  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_v2.json"
  154  nano prepare_bulk.py
  155  python3 prepare_bulk.py
  156  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X POST "https://localhost:9200/news/_bulk" -H "Content-Type: application/json" --data-binary "@News_Category_Dataset_bulk.json"
  157  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty"
  158  curl -u elastic:c+vdv5FUzys5hft5*8Fs -k -X GET "https://localhost:9200/news/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match": {
      "headline": "Trump"
    }
  }
}'
  159  history
root@elasticSrv1:/home/eleve#

```
