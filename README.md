
import json

# Définir une fonction pour charger les contacts depuis un fichier
def charger_contacts():
    try:
        with open("contacts.json", "r") as fichier:
            contacts = json.load(fichier)
    except FileNotFoundError:
        contacts = []
    return contacts

# Définir une fonction pour enregistrer les contacts dans un fichier
def enregistrer_contacts(contacts):
    with open("contacts.json", "w") as fichier:
        json.dump(contacts, fichier, indent=4)

# Définir une fonction pour ajouter un contact
def ajouter_contact(contacts, nom, numero, email, adresse):
    contact = {
        "Nom": nom,
        "Numéro": numero,
        "Email": email,
        "Adresse": adresse
    }
    contacts.append(contact)
    enregistrer_contacts(contacts)

# Définir une fonction pour mettre à jour un contact
def mettre_a_jour_contact(contacts, nom, nouveau_numero, nouvel_email, nouvelle_adresse):
    for contact in contacts:
        if contact["Nom"] == nom:
            contact["Numéro"] = nouveau_numero
            contact["Email"] = nouvel_email
            contact["Adresse"] = nouvelle_adresse
    enregistrer_contacts(contacts)

# Définir une fonction pour supprimer un contact
def supprimer_contact(contacts, nom):
    contacts = [contact for contact in contacts if contact["Nom"] != nom]
    enregistrer_contacts(contacts)

# Définir une fonction pour rechercher un contact par nom ou numéro de téléphone
def rechercher_contact(contacts, terme):
    resultat = []
    for contact in contacts:
        if terme in contact["Nom"] or terme in contact["Numéro"]:
            resultat.append(contact)
    return resultat

# Fonction principale
def main():
    contacts = charger_contacts()

    while True:
        print("1. Ajouter un contact")
        print("2. Mettre à jour un contact")
        print("3. Supprimer un contact")
        print("4. Rechercher un contact")
        print("5. Afficher la liste complète des contacts")
        print("6. Quitter")

        choix = input("Choisissez une option : ")

        if choix == "1":
            nom = input("Nom du contact : ")
            numero = input("Numéro de téléphone : ")
            email = input("Adresse e-mail : ")
            adresse = input("Adresse : ")
            ajouter_contact(contacts, nom, numero, email, adresse)
            print("Contact ajouté avec succès.")

        elif choix == "2":
            nom = input("Nom du contact à mettre à jour : ")
            nouveau_numero = input("Nouveau numéro de téléphone : ")
            nouvel_email = input("Nouvelle adresse e-mail : ")
            nouvelle_adresse = input("Nouvelle adresse : ")
            mettre_a_jour_contact(contacts, nom, nouveau_numero, nouvel_email, nouvelle_adresse)
            print("Contact mis à jour avec succès.")

        elif choix == "3":
            nom = input("Nom du contact à supprimer : ")
            supprimer_contact(contacts, nom)
            print("Contact supprimé avec succès.")

        elif choix == "4":
            terme = input("Entrez le nom ou le numéro de téléphone à rechercher : ")
            resultats = rechercher_contact(contacts, terme)
            if resultats:
                print("Résultats de la recherche :")
                for contact in resultats:
                    print(contact)
            else:
                print("Aucun résultat trouvé.")

        elif choix == "5":
            print("Liste complète des contacts :")
            for contact in contacts:
                print(contact)

        elif choix == "6":
            print("Au revoir!")
            break

if __name__ == "__main__":
    main()
