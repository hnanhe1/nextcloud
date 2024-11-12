Voici les étapes pour configurer un serveur, en incluant la création d'un utilisateur et la configuration de l'authentification SSH par clé :

### Étape 1: Connexion au Serveur

1. **Connectez-vous à votre serveur** via SSH en utilisant un compte ayant des privilèges sudo :
   ```bash
   ssh user@votre_serveur_ip
   ```

### Étape 2: Création d'un Nouvel Utilisateur

2. **Créer un nouvel utilisateur** :
   - Remplacez `nouvel_utilisateur` par le nom souhaité :
   ```bash
   sudo adduser nouvel_utilisateur
   ```
   - Suivez les instructions pour définir le mot de passe et remplir d'autres informations.

3. **Ajouter l'utilisateur au groupe sudo** (facultatif, pour donner des privilèges d'administration) :
   ```bash
   sudo usermod -aG sudo nouvel_utilisateur
   ```

### Étape 3: Configuration de SSH par Clé

4. **Générer une clé SSH sur votre machine locale** (si ce n'est pas déjà fait) :
   ```bash
   ssh-keygen -t rsa -b 4096 -C "votre.email@example.com"
   ```
   - Acceptez le chemin par défaut et entrez une phrase de passe si souhaité.

5. **Copier la clé publique vers le serveur** :
   - Utilisez `ssh-copy-id` pour copier la clé publique :
   ```bash
   ssh-copy-id nouvel_utilisateur@votre_serveur_ip
   ```
   - Entrez le mot de passe de l'utilisateur lorsque cela est demandé.

   **Alternativement**, vous pouvez copier la clé manuellement :
   - Affichez la clé publique :
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   - Connectez-vous à votre serveur et éditez (ou créez) le fichier `~/.ssh/authorized_keys` :
   ```bash
   mkdir -p ~/.ssh
   nano ~/.ssh/authorized_keys
   ```
   - Collez la clé publique dans ce fichier et enregistrez.

6. **Ajuster les permissions** :
   - Assurez-vous que les permissions du dossier `.ssh` et du fichier `authorized_keys` sont correctes :
   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

### Étape 4: Désactiver l'Authentification par Mot de Passe (Facultatif)

7. **Modifier la configuration SSH pour désactiver l'authentification par mot de passe** (optionnel, mais recommandé) :
   - Éditez le fichier de configuration SSH :
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
   - Trouvez et modifiez les lignes suivantes :
   ```
   PasswordAuthentication no
   ```
   - Redémarrez le service SSH pour appliquer les modifications :
   ```bash
   sudo systemctl restart ssh
   ```

### Conclusion

Vous avez maintenant configuré un nouvel utilisateur sur votre serveur et configuré l'authentification SSH par clé. Vous pouvez vous connecter avec la clé SSH sans avoir à entrer un mot de passe. Si vous avez d'autres questions ou besoin d'assistance, n'hésitez pas à demander !
