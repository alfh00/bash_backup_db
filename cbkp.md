```bash

# Configuration de la base de données

DB_USER="votre_utilisateur"
DB_PASS="votre_mot_de_passe"
DB_NAME="votre_base_de_donnees"

# Configuration du serveur distant

DISTANT_USER="utilisateur_distant"
DISTANT_SERVER="adresse_du_serveur_distant"
DISTANT_PATH="/chemin/vers/destination/"

# Répertoire local de sauvegarde

LOCAL_BACKUP_DIR="/chemin/vers/sauvegarde_locale"
DATE=$(date +"%Y-%m-%d")
BACKUP_FILE="$LOCAL_BACKUP_DIR/$DB_NAME-$DATE.sql"

# Créez le répertoire local de sauvegarde

mkdir -p $LOCAL_BACKUP_DIR

# Effectuez la sauvegarde avec mysqldump

mysqldump -u $DB_USER -p$DB_PASS $DB_NAME > $BACKUP_FILE

# Vérifiez si la sauvegarde locale a réussi

if [ $? -eq 0 ]; then
echo "Sauvegarde locale réussie : $BACKUP_FILE"

# Transférez la sauvegarde vers le serveur distant avec SCP

scp $BACKUP_FILE $DISTANT_USER@$DISTANT_SERVER:$DISTANT_PATH

# Vérifiez si le transfert a réussi

if [ $? -eq 0 ]; then
echo "Transfert vers le serveur distant réussi"
else
echo "Échec du transfert vers le serveur distant"
fi
else
echo "Échec de la sauvegarde locale"
fi

# Nettoyez les sauvegardes locales plus anciennes si nécessaire (par exemple, conservez les 7 derniers jours)

find $LOCAL_BACKUP_DIR -type f -name "\*.sql" -mtime +7 -exec rm {} \;
```
