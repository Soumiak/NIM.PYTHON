# NIM.PYTHON
kHELADI SOUMIA 201500008773
AHMEDZAID WISSEM 201500008986
BOUDJATAT ASMA 201500002038
DABOUZ LYDIA 201500009120
import random
import sys
import json

global nbTas
global tas
global joueurs
global tour


def afficher(maxPierre, nbTas, tas):
	print('\n')
	print('TOUR NUMERO : ' + str(tour) + '\n')
	for i in range(0, nbTas):
		message = str(i+1) + ' |  '

		for j in range(0, tas[i]):
			message += '*'

		for j in range(tas[i]+1, maxPierre + 3):
			message += ' '

		message += '| ' + str(tas[i])
		print(message)



def joue(joueur, tour):
	message = '\nA \'' + joueur + '\' de jouer ("numTas"-"nmbreDePierre") :   '

	coup = raw_input(message)

	return coup



def calcul(joueur, coup):
	i = 0

	while(i < len(coup) and coup[i] != '-'): 
		i += 1

	if(coup[:i].isdigit()):
		numTas = int(coup[:i]) - 1
	else:
		print('\nCOUP NON PERMIS\n')
		return 0

	if(numTas < 0 or numTas > nbTas or i == len(coup) or not numTas.isdigit()):
		print('\nCOUP NON PERMIS\n')
		return 0
	
	if(nbPierre.isdigit()):
		nbPierre = int(coup[(i+1):])

	if(tas[numTas] >= nbPierre and nbPierre > 0 ):
		tas[numTas] -= nbPierre
		return 1
	else:
		print('\nCOUP NON PERMIS\n')
		return 0


def verifier():
	jouer = 0
	dernier = 0
	i = 0
	
	while (not jouer and i < nbTas):
		if(tas[i] > 1):
			jouer = 1
		elif(tas[i] == 1):
			if(dernier == 0):
				dernier = 1
				i += 1
			else:
				jouer = 1
		else:
                        
			i+= 1

	return jouer


def finDePartie(joueur):
	score = 0

	for i in range(1, tour+1):
		score += i * (10**i)

	message = joueur + ' a gagné la partie et score : ' + str(score)
	print('\n\n\n' + message)

	return score


def chercherJoueur(joueur):
	with open('scores.json', 'r') as f:
		datas = json.load(f)

	found = 0

	for i in xrange(len(datas)) :
		if(joueur == datas[i]['nom']):
			found = 1
			joueur = datas[i]

	if found:
		print('\nMeilleur score :' + str(joueur['meilleur_score']))
		print('Dernier score :' + str(joueur['dernier_score']))

		return joueur
	else:
		nvJoueur = {
				"nom": joueur,
				"meilleur_score": 0,
				"dernier_score": 0
			}
		
		return nvJoueur


nbTas = random.randint(3, 7)
tas = []
for i in range(0, nbTas):
	tas.append(random.randint(5, 23))

joueur1 = raw_input('\nJoueur1 : Veuillez entrer votre nom  : ')
joueur1_dict = chercherJoueur(joueur1)

joueur2 = raw_input('\n\nJoueur2 : Veuillez entrer votre nom  : ')
joueur2_dict = chercherJoueur(joueur2)

joueurs = [joueur1, joueur2]

jouer = 1
i = 0
tour = 1

print('\nDEBUT DE LA PARTIE ____________')

while jouer:
	tourJoueur = joueurs[i % 2]
	maxPierre = max(tas)
	afficher(maxPierre, nbTas, tas)

	coup = joue(tourJoueur, tour)
	result = calcul(tourJoueur, str(coup))

	if (result == 1):
		jouer = verifier()
		
		if jouer == 1:
			i += 1
			tour += 1
		else:
			afficher(1, nbTas, tas)
			score = finDePartie(tourJoueur)

			if((i % 2) == 0):
				joueur1_dict['dernier_score'] = score
				if joueur1_dict['meilleur_score'] < score:
					joueur1_dict['meilleur_score'] = score

				joueur2_dict['dernier_score'] = 0
			else:
				joueur2_dict['dernier_score'] = score
				if joueur2_dict['meilleur_score'] < score:
					joueur2_dict['meilleur_score'] = score

				joueur1_dict['dernier_score'] = 0

			with open('scores.json', 'r') as f:
				datas = json.load(f)

			for i in xrange(len(datas)):
				if datas[i]['nom'] == joueur1:
					datas.pop(i)
					break

			for i in xrange(len(datas)):
				if datas[i]['nom'] == joueur2:
					datas.pop(i)
					break
					
			datas.append(joueur1_dict)
			datas.append(joueur2_dict)

			open("scores.json", "w").write(
		    json.dumps(datas, sort_keys=True, indent=4, separators=(',', ': ')))
        [
    {
        "dernier_score": 76543210,
        "meilleur_score": 76543210,
        "nom": "soumia"
    },
    {
        "dernier_score": 0,
        "meilleur_score": 543210,
        "nom": "asma"
    },
    {
        "dernier_score": 1209876543210,
        "meilleur_score": 1209876543210,
        "nom": "Wissem"
    },
    {
        "dernier_score": 0,
        "meilleur_score": 0,
        "nom": "lydia"
    }

