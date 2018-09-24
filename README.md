# data-analysis-pandas-matplotlib-python
Use pandas for the arrays and matplotlib for the graphs

import pandas



#Découverte du Datataset

import csv
f = open("guns.csv", "r",encoding ="utf-8")
reader = csv.reader(f)
data =list(reader)

print(data[0:5]) #data[:5]

headers = data[0]

data = data[1:]

#print(headers)
print(data[0:5])




#Compter le nombre de décès par arme à feu aux USA  par année 

years =[]

years= [y[1] for y in data]
 

year_counts ={}

for yc in years: 
  if yc in year_counts:
    year_counts[yc] += 1
  else:
    year_counts[yc] = 1


annee = pandas.DataFrame(year_counts, index =["Nombre de décès aux USA"]).transpose()


#Autre méthode:
year_counts1 ={}
for yc in years: 
  if not yc in year_counts1:
    year_counts1[yc] = 0 # on crée le dictionnaire
  year_counts1[yc] += 1 #on incrémente.




#Explorer les décès aux USA par arme à feu  par mois et année

import datetime

dates =[]

dates =[datetime.datetime(year= int(date[1]), month = int(date[2]), day = 1) for date in data]

#print(dates[0:5])

date_counts = {}

for date in dates:
  if date in date_counts:
    date_counts[date]+=1
  else:
    date_counts[date]= 1

print(date_counts) 



#Explorer les décès aux USA par arme à feu par origine et sexe

sex_counts ={}

for s in data:
  sex = s[5]
  if sex in sex_counts:
    sex_counts[sex]+=1
  else:
    sex_counts[sex] = 1



sex = pandas.DataFrame(sex_counts, index =["Nombre de décès aux USA"]).transpose()

ethnic_counts ={}



for o in data:
  origine = o[7]
  if not origine in ethnic_counts:
    ethnic_counts[origine]= 0
  ethnic_counts[origine]+=1


origine = pandas.DataFrame(ethnic_counts,index =["Nombre de décès aux USA"]).transpose().sort_index(axis = 0, ascending = True)

tableau_final = pandas.concat([annee,sex,origine], sort =True)


print(tableau_final)




#Dataset population (census.csv)
import csv
with open("census.csv", "r") as f:
  reader = csv.reader(f)
  census = list(reader)

import pandas as p

visuel = p.DataFrame(census).transpose()

#print(census)
#print(visuel)




# Exploration des ratios de décès en fonction de l'origine (doc census.csv).

mapping ={}

mapping["Asian/Pacific Islander"] = int (census[1][14]) + int(census[1][15])

mapping["Black"] = int(census[1][12])

mapping["Native American/Native Alaskan"] = int(census[1][13])

mapping["Hispanic"] = int(census[1][11])

mapping["White"] = int(census[1][10])

tableau_mapping= pandas.DataFrame(mapping, index = ["Référence du nombre de décès par origine"]).transpose()

print(tableau_mapping)




#Filtrer par la nature du décès, ou par le sexe, ou par l'année

intents = [i[3]for i in data]
ethnic_group = [e[7]for e in data]
years1 = [y[1] for y in data]
sex = [s[5] for s in data]

#print(set(intents))

homicide_counts ={}

#enumerate() est comme items() pour le dictionnaire, sauf qu'il s'applique pour les listes, il permet de récuperer l'index et la valeur de la liste.

for i, v in enumerate(ethnic_group):
  if intents[i] == "Homicide":
    if v in homicide_counts:
      homicide_counts[v]+=1
    else:
      homicide_counts[v] = 1

#print(homicide_counts) 


#fonction pour compter le nombre d'éléments de la première liste en fonction de la seconde liste.

def comptage(liste1, liste2,valeur):
  dico= {}
  for i, v in enumerate(liste1):
    if liste2[i] == valeur:
      if v in dico:
        dico[v]+= 1
      else:
        dico[v]= 1
  return dico

accidentals = comptage(ethnic_group,intents,'Accidental')

suicides= comptage(ethnic_group, intents, 'Suicide')

undetermined = comptage(ethnic_group, intents, 'Undetermined')

na = comptage(ethnic_group, intents, 'NA')

m = comptage(ethnic_group, sex, "M")

f = comptage(ethnic_group, sex, "F")

a2012 = comptage(ethnic_group,years1,"2012")
a2013 = comptage(ethnic_group, years1, "2013")
a2014 = comptage (ethnic_group, years1, "2014")


#Création de tableaux à partir d'un dictionnaire

homicides_columns = pandas.DataFrame(homicide_counts, index =["Homicides"]).transpose()

suicides_columns = pandas.DataFrame(suicides, index =["Suicides"]).transpose()

#df = pandas.merge(homicides_columns,suicides_columns, left_index = True,right_index=True)

accidentals_columns = pandas.DataFrame(accidentals, index =["Accidentals"]).transpose()


undetermined_columns = pandas.DataFrame(undetermined, index =["Undetermined"]).transpose()


na_columns = pandas.DataFrame(na, index =["NA"]).transpose()

f_columns = pandas.DataFrame(f, index =["F"]).transpose()

m_columns = pandas.DataFrame(m, index =["M"]).transpose()

a2012_columns= pandas.DataFrame(a2012, index =["2012"]).transpose()

a2013_columns= pandas.DataFrame(a2013, index =["2013"]).transpose()

a2014_columns= pandas.DataFrame(a2014, index =["2014"]).transpose()

array_final = pandas.concat([homicides_columns, suicides_columns,accidentals_columns, undetermined_columns,na_columns,f_columns,m_columns,a2012_columns,a2013_columns,a2014_columns], axis = 1, sort = True).sort_index(axis = 0, ascending = True)

#array_final est la fusion de plusieurs tableaux entre eux grâce concat()).


print(array_final)





#Exploration du nombre de décès en fonction de l'origine, la nature du décès et de l'année aux USA.

def comptage_filtrant(liste1, liste2, valeur, liste3, valeur2):
  info ={}
  for i,v in enumerate(liste1):
    if liste2[i]== valeur:
      if liste3[i] == valeur2:
        if v+"_"+valeur in info:
          info[v+"_"+valeur]+=1
        else:
          info[v+"_"+valeur] = 1
        
  return info


liste_nature = list(set(intents)) 
liste_dico = []

liste_dico = [pandas.DataFrame(comptage_filtrant(ethnic_group, intents, nature, years1, "2012"), index=["Nombres_de_décès aux USA en 2012"]).transpose() for nature in liste_nature]

liste_dico1 = [pandas.DataFrame(comptage_filtrant(ethnic_group, intents, nature, years1, "2013"), index = ["Nombres_de_décès aux USA en 2013"]).transpose()  for nature in liste_nature]

liste_dico2 = [pandas.DataFrame(comptage_filtrant(ethnic_group, intents, nature, years1, "2014"), index = ["Nombres_de_décès aux USA en 2014"]). transpose() for nature in liste_nature]

#print(len(liste_dico))
#print(len(liste_dico1))
#print(len(liste_dico2))



#Visualisation sous forme de tableau le nombre de décès en fonction de l'origine, la nature du décès et de l'année.


tb12 = pandas.concat([liste_dico[0],liste_dico[1],liste_dico[2],liste_dico[3],liste_dico[4]], sort =True)

tb13 = pandas.concat([liste_dico1[0],liste_dico1[1],liste_dico1[2],liste_dico1[3],liste_dico1[4]], sort =True)

tb14 = pandas.concat([liste_dico2[0],liste_dico2[1],liste_dico2[2],liste_dico2[3],liste_dico2[4]], sort =True)

print(tb12)
print(tb13)
print(tb14)



#Exploration du ratio des décès aux USA en fonction l'origine des individus 
ratio ={}

for key, value in ethnic_counts.items():
  for cle, valeur in mapping.items():
    if key == cle:
      rratio = (value/valeur)*100000 #on analyse pour 100000 personnes.
      ratio [key] = rratio

ratio_origine = pandas.DataFrame(ratio, index = ["Ratio du nombre de décès aux USA en fonction de l'origine"]).transpose().sort_index(axis = 0, ascending = True)

print(ratio_origine)

#Autre methodes: 
ratio1={}
for k,v in ethnic_counts.items():
  ratio1[k] = (v/mapping[k])*100000




#Exploration des ratios par origine et par la nature du décès, ou par sex ou par année aux USA.



homicide_ratio = {}

for cle, valeur in homicide_counts.items():
  homicide_ratio[cle]=(valeur/mapping[cle])*100000


ratio_par_homicide = pandas.DataFrame(homicide_ratio, index = ['Ratios origine par homicide']).transpose()



#Le nombre de décès par homicide est la plus élevée chez la population noire tandis qu'elle est plus faibles chez la population asiatique ET d'origine pacifique.


def ratio(dictionnaire):
  dico_ratio={}
  for cle, valeur in dictionnaire.items():
    dico_ratio[cle]=(valeur/mapping[cle])*100000
  return dico_ratio 

suicides_ratio = pandas.DataFrame(ratio(suicides), index =["Ratios origine par suicide"]).transpose() 


accidentals_ratio = pandas.DataFrame(ratio(accidentals), index = ["Ratios origine par accidents"]).transpose()

undetermined_ratio =pandas.DataFrame (ratio(undetermined), index = ["Ratios origine par undetermined"]).transpose()

na_ratio = pandas.DataFrame (ratio(na), index =["Ratios origine par NA"]).transpose()

masculin_ratio = pandas.DataFrame (ratio(m), index =["Ratios par origine hommes"]).transpose()

feminin_ratio = pandas.DataFrame (ratio(f), index =["Ratios par origine femmes"]).transpose()

a2012_ratio = pandas.DataFrame (ratio(a2012), index =["Ratios origine pour 2012"]).transpose()

a2013_ratio = pandas.DataFrame (ratio(a2013), index =["Ratios origine pour 2013"]).transpose()

a2014_ratio = pandas.DataFrame (ratio(a2014), index =["Ratios origine pour 2014"]).transpose()



array_ratio = pandas.concat([ratio_par_homicide,suicides_ratio, accidentals_ratio, undetermined_ratio, na_ratio,masculin_ratio,feminin_ratio, a2012_ratio, a2013_ratio, a2014_ratio], axis = 1, sort = True).sort_index(axis = 0, ascending = True)

print(array_ratio)

print(array_ratio["Ratios origine pour 2012"])

print(array_ratio["Ratios par origine hommes"])



#Visualisation graphique de l'évolution du nombre de décès aux USA selon le sexe et l'année 2014.

import matplotlib.pyplot as plt

def nuages_de_points1(x, y1, y2, labelx,labely,titre):
  import matplotlib.pyplot as plt
  plt.scatter(x, y1, c = 'red' )
  plt.scatter(x, y2, c = 'yellow')
  plt.xlabel(labelx)
  plt.ylabel(labely)
  
  plt.text(2.5, 10, "Asian/Pacific Islander",fontsize= 8)
  
  plt.annotate("Black",xy = (18.9, 6), xytext = (19, 24), arrowprops = {'facecolor': 'yellow', 'edgecolor': 'red','width': 3, 'headwidth': 15,'shrink': 0.1}, color = 'black', backgroundcolor = "white")
  plt.annotate("Black",xy = (19, 47), xytext = (19, 47), backgroundcolor = "white")
  plt.title(titre, fontsize= 11)
  plt.legend()
  plt.grid(True)
  return plt.savefig("plot5.png")


  
graph2 = nuages_de_points1(array_ratio["Ratios origine pour 2014"],array_ratio["Ratios par origine femmes"],array_ratio["Ratios par origine hommes"],"Ratios origine pour 2014","Ratios","Etude de la corrélation du nombre de décès aux USA en fonction du sexe")


print(graph2)


#Visualisation graphique de l'évolution du nombre de décès aux USA chaque année (2012,2013,2014) en fonction du sexe.

def nuages_de_points2(x, y1, y2,y3,labelx,labely,titre): 
  import matplotlib.pyplot as plt
  plt.scatter(x, y1, c = 'red', marker = "v" )
  plt.scatter(x, y2, c = 'blue',marker= "*")
  plt.scatter(x, y3, c = "green", marker ="x")
  plt.xlabel(labelx, fontsize=8)
  plt.ylabel(labely,fontsize=8)
  plt.legend(prop={'size':6.5})
  plt.title(titre, fontsize= 6.5)
  plt.grid(True)
  return plt.savefig("plot7.png")

plt.figure(1) #figure permet de classer l'ordre d'affichage des différentes figures.
plt.subplot(1,2,1)

nuages_de_points2(array_ratio["Ratios par origine hommes"],array_ratio["Ratios origine pour 2012"],array_ratio["Ratios origine pour 2013"],array_ratio["Ratios origine pour 2014"], "Proportions de décès aux USA chez les hommes", "Proportions de décès chaque année", "Corrélation du nb décès aux USA chez les hommes" )

plt.subplot(1,2,2)

nuages_de_points2(array_ratio["Ratios par origine femmes"],array_ratio["Ratios origine pour 2012"],array_ratio["Ratios origine pour 2013"],array_ratio["Ratios origine pour 2014"], " et chez les femmes", "Proportions de décès chaque année", "et chez les femmes d'origines éthniques différentes chaque année")

plt.gcf().subplots_adjust(left = 0.107, bottom = 0.2, right = 0.95, top = 0.9, wspace = 0.26, hspace = 0.35)

plt.savefig('plot7.png')



#Visualisation de la proportion de décès aux USA en fonction de l'origine éthnique chaque année (2012,2013,2014) ou selon le sexe.

origine_people = sorted(list(set(ethnic_group )))

def camembert3(table1,table2, titre, taille_titre):
  import matplotlib.pyplot as plt
  plt.pie(table1, labels= table2, autopct="%1.2f pourcents",textprops={'fontsize': 6})
  plt.legend(loc ="best",prop={'size':4})
  plt.title(titre,fontsize = taille_titre)
  return plt.savefig('plot6.png')

#selon le sexe


plt.figure(2)
plt.subplot(1,2,1)

camembert3(array_ratio["Ratios par origine hommes"],origine_people,"Proportions de décès aux USA chez les hommes",9)

plt.subplot(1,2,2)

camembert3(array_ratio["Ratios par origine femmes"],origine_people,"Proportions de décès aux USA chez les femmes",9)

plt.gcf().subplots_adjust(left = 0.093, bottom = 0.2, right = 0.95, top = 0.9, wspace = 0.37, hspace = 0.35)

plt.savefig('plot8.png')


#selon l'année

plt.figure(3)

plt.gcf().subplots_adjust(left = 0.107, bottom = 0.2, right = 0.95, top = 0.9, wspace = 0.45, hspace = 0.35)

plt.subplot(1,2,1)

camembert3(array_ratio["Ratios origine pour 2014"],origine_people,"Proportions de décès aux USA en 2014",9)

  
plt.subplot(2,2,2)

camembert3(array_ratio["Ratios origine pour 2013"],origine_people,"Proportions de décès aux USA en 2013",9)


plt.subplot(2,2,4)

camembert3(array_ratio["Ratios origine pour 2012"],origine_people,"Proportions de décès aux USA en 2012",9)



plt.savefig('plot6.png')


