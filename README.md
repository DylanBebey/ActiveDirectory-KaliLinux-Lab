# ActiveDirectory-KaliLinux-Lab
Projet de mise en place d’un domaine Active Directory (Windows Server) et tentative d’intégration d’un poste client Kali Linux.

# Intégration d’un poste Kali Linux dans un domaine Active Directory

## 🎯 Objectif du projet
Mettre en place un domaine Active Directory avec Windows Server (rôle AD DS + DNS), créer et gérer des utilisateurs/groupes, puis tenter l’intégration d’une VM Kali Linux au domaine `lab.local`.  
Le projet documente la configuration, les tests réseau/DNS, les difficultés rencontrées et les solutions apportées.

---

## ⚙️ Environnement technique
- **VirtualBox** : Virtualisation
- **Windows Server 2019/2022** (contrôleur de domaine)
  - Domaine : `lab.local`
  - Hôte : `DC-TEST`
  - Rôles installés : AD DS, DNS
- **Kali Linux** (poste client)
- Réseau VirtualBox **Host-Only** (192.168.56.0/24)

---

## 🚀 Étapes réalisées
1. **Installation du DC**
   - Installation Windows Server en VM
   - Rôles AD DS + DNS installés
   - Promotion en contrôleur de domaine `lab.local`
   - Renommage en `DC-TEST`

2. **Organisation Active Directory**
   - Création de l’OU `IT`
   - Création des utilisateurs : `Alice` et `Bob`
   - Création du groupe `IT_Group`
   - Ajout d’Alice dans `IT_Group`

3. **Préparation de Kali**
   - Vérification ping vers `192.168.56.101`
   - Correction de `/etc/resolv.conf` (remplacement du mauvais DNS `10.0.2.3` par `192.168.56.101`)
   - Tests `nslookup lab.local` et `ping lab.local` → OK

4. **Tentative de jointure**
   - `realm discover lab.local` → domaine détecté
   - `realm join -U Administrator lab.local` → erreurs rencontrées
     - Paquets manquants (`sssd`, `libnss-sss`, `libpam-sss`, `adcli`)
     - Service `sssd` inactif
     - Keytab `/etc/krb5.keytab` absent (normal tant que la jointure échoue)
   - Jointure mise en pause (contrainte espace disque)

---

## ✅ Résultats obtenus
- Domaine `lab.local` opérationnel
- OU, utilisateurs et groupes créés et fonctionnels
- Kali Linux détecte le domaine (DNS corrigé, `realm discover` OK)
- La jointure complète sera poursuivie plus tard

---

## 📚 Compétences pratiquées
- **Windows Server** : installation AD DS/DNS, création OU, users, groupes
- **Réseaux VirtualBox** : Host-Only, isolation réseau
- **Linux (Kali)** : DNS (`resolv.conf`), realmd, sssd, Kerberos
- **Diagnostic** : tests ping/nslookup, débogage DNS et services
- **Méthodologie** : gestion d’incidents, documentation et rapport technique

---

## ⚠️ Difficultés rencontrées
- Mauvaise configuration DNS sur Kali (résolu en pointant vers `192.168.56.101`)
- Paquets manquants pour la jointure realm
- Service `sssd` inactif tant que la jointure n’est pas finalisée
- Limitation d’espace disque empêchant d’aller jusqu’au bout

---

## 📝 Livrable
👉 Rapport complet disponible : [📄 Projet Active Directory & Kali Linux](./Projet1%20Active%20Directory.pdf)

---

## 👤 Auteur
Projet réalisé par BEBEY NZEKE Dylan Chriist
Étudiant en Bachelor 3 en Administration d'Infrastructure Sécurisée à l'ECE de Paris
