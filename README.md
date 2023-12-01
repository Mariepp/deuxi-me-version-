# deuxi-me-version
    
import random

difficulty =1
carte = [
    [1, 2, 3, 4, 5, 6],
    [7, 8, 9, 10, 11, 12],
    [13, 14, 15, 16, 17, 18]
]
personnage = {'char': 'o', 'x': 0, 'y': 0}          #x , y coordonnées dans un repère orthonormé en 0,0 en haut a gauche

dictionnaire = {
    1: ' ',
    2: '#',
    3: ' ',
    4: ' ',
    5: ' ',
    6: '#',
    7: ' ',
    8: ' ',
    9: ' ',
    10:' ',
    11:'#',
    12:' ',
    13:' ',
    14:'#',
    15:' ',
    16:' ',
    17:' ',
    18:'#',
    
}

score=0


def display_map(m, d):
    for ligne in m: #  pour chaque ligne dans la matrice m
        for colonne in ligne: # pour chaque element de chaque ligne (élement 1 fait partie de la premiere colonne etc..)
            if colonne in d: 
                print(d[colonne], end="") # on affiche les élements de chaque ligne 1 par 1 (en réferant au dictionnaire)
            else:
                print(colonne, end="") 
        print()  # Pour passer à la ligne après avoir affiché une ligne complète (retour à la ligne : "\n" automatique avec print)









def create_perso(cordonnees): # coordonnées sous forme de tuple (x,y)
    personnage = {
        'char': 'o',
        'x': cordonnees[0],
        'y': cordonnees[1]
    }
    return personnage

def display_map_and_char(m, d, p): # m la carte, d le dictionnaire, et p le personnage
    map_copy = [ligne[:] for ligne in m] # on créé une copie de la map m (matrice) pour ne pas faire référence à une matrice déjà existante et éviter les erreurs d'affichage
    map_copy[p['y']][p['x']] = p['char']
    display_map(map_copy, d)  # Affiche la carte avec le personnage positionné
   
#def update_p(letter, p): # lettre 'z' = input du déplacement , p le personnage
    #if letter == 'z':
        #p['y'] -= 1  # Déplacement vers le haut (diminution de la ligne)
    #elif letter == 's':
        #p['y'] += 1  # Déplacement vers le bas (augmentation de la ligne)
    #elif letter == 'q':
        #p['x'] -= 1  # Déplacement vers la gauche (décalage dans la meme liste (ligne))
    #elif letter == 'd':
        #p['x'] += 1  # Déplacement vers la droite (augmentation dans la meme liste)

    
def recup(p): # p : personnage
    global score  # Déclarer 'score' comme variable globale

    m = create_objects(difficulty, dictionnaire)

    while True:
        display_map_char_and_objects2(carte, dictionnaire, p, m)
        d = input("Où va-t-on ?")
        update_pmur(d, p, carte, dictionnaire)


        # Vérifier si la position actuelle du joueur correspond à un objet
        m=update_objects(personnage,m)
        
        

        
    
#def update_p2(letter, p, m):
    #hauteurdecarte = len(m)
    #largeurdecarte = len(m[0])

    #if letter == 'z' and p['y'] > 0:  # Déplacement vers le haut
        #p['y'] -= 1
    #elif letter == 's' and p['y'] < hauteurdecarte - 1:  # Déplacement vers le bas
        #p['y'] += 1
    #elif letter == 'q' and p['x'] > 0:  # Déplacement vers la gauche
        #p['x'] -= 1
    #elif letter == 'd' and p['x'] < largeurdecarte - 1:  # Déplacement vers la droite
        #p['x'] += 1
        
        
        
def update_pmur(letter, p, m, d):
    hauteurdecarte = len(m)
    largeurdecarte = len(m[0])

    nouveau_x, nouveau_y = p['x'], p['y']
    with open('control_up.txt','r') as control_up:
        with open('control_down.txt', 'r') as control_down:
            with open('control_left.txt', 'r') as control_left:
                with open('control_right.txt', 'r') as control_right:
                    with open('control_bomb.txt', 'r') as control_bomb:
                        contenuup=control_up.read()
                        contenudown=control_down.read()
                        contenuleft=control_left.read()
                        contenuright=control_right.read()
                        contenubomb=control_bomb.read()
                        if letter not in [contenuup,contenudown,contenuleft,contenuright,contenubomb,"pause"]:
                            recup(personnage)
        

                        if letter == contenuup:
                            nouveau_y -= 1
                        elif letter == contenudown:
                            nouveau_y += 1
                        elif letter == contenuleft:
                            nouveau_x -= 1
                        elif letter == contenuright:
                            nouveau_x += 1
                        elif letter == contenubomb:
                            delete_all_walls(dictionnaire,personnage)
                        elif letter =="pause":
                            run()
    
    
                        if 0 <= nouveau_x < largeurdecarte and 0 <= nouveau_y < hauteurdecarte:
                            if d[m[nouveau_y][nouveau_x]] != "#":
                                p['x'], p['y'] = nouveau_x, nouveau_y

     
        
        
def start():
    recup(personnage)
    
def run():
    print("Que voulez vous faire ?")
    print("1 : Settings")
    print("2 : Play")
    print("3 : Score")
    d=input()
    if d =="1":
        settings()
    elif d=="2":
        start()
    elif d=="exit":
        quit()
    elif d=="3":
        print("Votre score est : ", score)
        run()
    else:
        run()
    return


def changehero(): # settings 1 : permet de changer d'avatar !
    nouveau_personnage = input("Choisissez le personnage à contrôler")
    if nouveau_personnage != "#" and nouveau_personnage != "." and len(nouveau_personnage)==1:
        personnage['char']= str(nouveau_personnage)
    else:
        print("!  Vous ne pouvez pas choisir cet avatar  !") # Erreur dans le code si l'avatar à contrôler est aussi représentant des murs ou des points à récolter 
        changehero()

def settings():
    print("Que voulez-vous modfier ?")
    print("1 : Character")
    print("2 : Level")
    print("3 : Controls")
    d = input()
    if d =="1": changehero(), settings()
    elif d=="2": changelevel(), settings()
    elif d=="3": changekeys(), settings()
    elif d=="exit": run()
        
def changekeys():
    print("Quels contrôles souhaitez vous modifier ?")
    print("1 : Set Up")
    print("2 : Set Down")
    print("3 : Set Strafe left")
    print("4 : Set Strafe right")
    print("5 : Set Bomb!")
    print("6 : Default Settings")
    print("7 : Show my current controls")
    d=input()
    defaultup="z"
    defaultdown="s"
    defaultleft="q"
    defaultright="d"
    defaultbomb="b"
        
    if d=="1":
        with open('control_up.txt','w') as control_up:
            newkey=input("Set up key")
            control_up.write(newkey)
    elif d=="2":
        with open('control_down.txt', 'w') as control_down:
            newkey=input("Set down key")
            control_down.write(newkey)
    elif d=="3":
        with open('control_left.txt', 'w') as control_left:
            newkey=input("Set strafe left key")
            control_left.write(newkey)
    elif d=="4":
        with open('control_right.txt', 'w') as control_right:
            newkey=input("Set strafe right key")
            control_right.write(newkey)
    elif d=="5":
        with open('control_bomb.txt', 'w') as control_bomb:
            newkey=input("Set bombing key")
            control_bomb.write(newkey)
    elif d=="6":
        with open('control_up.txt','w') as control_up:
            control_up.write(defaultup)
        with open('control_down.txt', 'w') as control_down:
            control_down.write(defaultdown)
        with open('control_left.txt', 'w') as control_left:
            control_left.write(defaultleft)
        with open('control_right.txt', 'w') as control_right:
            control_right.write(defaultright)
        with open('control_bomb.txt', 'w') as control_bomb:
            control_bomb.write(defaultbomb)
            
        
        
        
    elif d=="7":
        with open('control_up.txt','r') as control_up:
            with open('control_down.txt', 'r') as control_down:
                with open('control_left.txt', 'r') as control_left:
                    with open('control_right.txt', 'r') as control_right:
                       with open('control_bomb.txt', 'r') as control_bomb:
                            contenuup=control_up.read()
                            contenudown=control_down.read()
                            contenuleft=control_left.read()
                            contenuright=control_right.read()
                            contenubomb=control_bomb.read()
                            print("Up :",contenuup)
                            print("Down :",contenudown)
                            print("Left :",contenuleft)
                            print("Right :",contenuright)
                            print("Bomb! :",contenubomb)
        
        



def changelevel():
    print("Set difficulty level")
    print("1: Easy")
    print("2: Medium")
    print("3: HARDCORE MODE")
    k = input()
    if k=="1":
        setdifficulty(1)
    if k=="2":
        setdifficulty(2)
    if k=="3":
        setdifficulty(3)


def setdifficulty(n):
    global difficulty
    difficulty = n*2


def create_objects(nb_objects, m):
    lignes = 3
    colonnes = 6
    available_positions = [(y, x) for y in range(lignes) for x in range(colonnes)]  # Liste des positions disponibles sur la carte

    objects_placed = set()  # Ensemble pour stocker les positions des objets placés

    while nb_objects > 0 and available_positions:
        random_index = random.randint(0, len(available_positions) - 1)
        y, x = available_positions[random_index]  # Sélection aléatoire d'une position disponible

        position = y * colonnes + x  # Calcul de la position dans la carte

        if m[position + 1] == ' ':  # Vérification si la position est vide dans la carte
            objects_placed.add((y, x))  # Ajout de la position dans les objets placés
            nb_objects -= 1  # Réduction du nombre d'objets restants
        available_positions.pop(random_index)  # Retrait de la position de la liste des positions disponibles

    return objects_placed  # Renvoyer l'ensemble des positions des objets placés


#def display_map_char_and_objects(m, d, p, objects):
    #map_copy = [ligne[:] for ligne in m]  # Création d'une copie de la carte pour éviter les modifications

    # Placement du personnage sur la carte copiée 
    #map_copy[p['y']][p['x']] = p['char'] 

    # Ajout des objets sur la carte copiée
    #for obj_pos in objects:
        #if isinstance(obj_pos, tuple) and len(obj_pos) == 2:  # Vérification que obj_pos est bien un tuple de deux éléments (ligne, colonne)
            #ligne, colonne = obj_pos
            #if map_copy[ligne][colonne] in d and d[map_copy[ligne][colonne]] == ' ':
                #map_copy[ligne][colonne] = '.'  # '.' représente un objet sur la carte

    # Affichage de la carte avec le personnage et les objets positionnés
    #display_map(map_copy, d)
    
def update_objects(p, objects): #{(x,y): ".", (x,y): " ", (x,y) : "."}
    player_pos = (p['y'], p['x'])  # Récupère la position actuelle du joueur sous forme de tuple
    global score
    if player_pos in objects:  # Vérifie si la position du joueur correspond à un objet
        objects.remove(player_pos) # Retire l'objet à la position du joueur
        score+=1
    return objects

    
def display_map_char_and_objects2(m,d,p, objects):  # Nouvelle fonction pour afficher les objets
        map_with_char = [ligne[:] for ligne in m]  # Copie de la carte avec le personnage

        # Placement des objets sur la carte
        for obj in objects:
            y, x = obj
            map_with_char[y][x] = '.'  # Remplacer '.' pour représenter les objets

        # Affichage de la carte avec le personnage et les objets
        display_map_and_char(map_with_char, dictionnaire,p)

def delete_all_walls(m, pos):
    lignes = 3
    colonnes = 6
    y, x = pos['y'], pos['x']  # Récupération de la position actuelle du personnage

    # Coordonnées des positions autour du personnage
    positions_around = [
        (y - 1, x), (y + 1, x), (y, x - 1), (y, x + 1),
        (y - 1, x - 1), (y - 1, x + 1), (y + 1, x - 1), (y + 1, x + 1)
    ]

    for y, x in positions_around:
        if 0 <= y < lignes and 0 <= x < colonnes:  # Vérification des limites de la carte
            position = y * colonnes + x + 1  # Calcul de la position dans la carte
            if m[position] == '#':  # Vérification si la position est un mur
                m[position] = ' '  # Remplacer le mur par un espace
    
    
                
                    
