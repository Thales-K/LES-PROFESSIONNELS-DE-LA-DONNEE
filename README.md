# ENQUETE LES PROFESSIONNELS DE LA DONNEE
## SOMMAIRE
- [Objectif du Projet](##Objectif-du-Projet)
- [Réalisation du projet](##Réalisation-du-projet)
  - [Extraction nettoyage et transformation des données](###Extraction-nettoyage-et-transformation-des-données)
  - [Visualisation](###Visualisation)
- [Analyse et Principaux résultats](##Analyse-et-Principaux-résultats)
  - [Caractéristiques de l'échantillon](###Caractéristiques-de-l'échantillon)
  - [Principaux enseignements et recommandations](###Principaux-enseignements-et-recommandations)



## Objectif du Projet
Ce projet utilise les données d'une enquête qui vise à comprendre les préférences, les tendances et les défis auxquels les professionnels des données sont confrontés. Il offre une interface conviviale avec des visualisations interactives pour faciliter l'exploration des données et la prise de décision. La base de données a été obtenue [ici](https://www.kaggle.com/datasets/ahmedmohamedibrahim1/data-professional-survey-breakdown)  

## Réalisation du projet
Il est réalisé avec Power bi desktop. 

### Extraction, nettoyage et transformation des données
Les données ont été nettoyées et transformées avec Power Query. De façon spécifique, il s'est agit de : 

- identifier les variables d'intérêt pour l'analyse ;
- transformer certaines variables ;
- corriger les types de variables incorrects ;
- corriger les ordres de grandeurs de certaines variables
- conserver uniquement les variables pertinentes pour l'analyse.

Cela a pu être effectuer grâce au code suivant : 


	let
	    Source = Excel.Workbook(File.Contents("CHEMIN\Power BI Project.xlsx"), null, true),
	    #"Data Professional Survey_Sheet" = Source{[Item="Data Professional Survey",Kind="Sheet"]}[Data],
	    #"Promoted Headers" = Table.PromoteHeaders(#"Data Professional Survey_Sheet", [PromoteAllScalars=true]),
	    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Identifiant", type text}, {"Email", type text}, {"Date d'interview (America/New_York)", type date}, {"Heure  d'interview  (America/New_York)", type time}, {"Naviagteur", type text}, {"Système", type text}, {"Ville", type text}, {"Pays", type text}, {"Référant", type text}, {"Durée de l'interview", type duration}, {"Q1 - Position Actuel", type text}, {"Q2 - Avez changé de carrière?", type text}, {"Q3 - Salaire actuel (en USD)", type text}, {"Q4 - Industrie?", type text}, {"Q5 - Langage de Programmation préféré", type text}, {"Q6 - A quel point Etes vous satisfait de votre salaire? ", Int64.Type}, {"Q6 - A quel point Etes vous satisfait de votre équilibre viepriéé/vie professionnel? ", Int64.Type}, {"Q6 - A quel point Etes vous satisfait de vos collègues? ", Int64.Type}, {"Q6 - A quel point Etes vous satisfait de votre hiérarchie? ", Int64.Type}, {"Q6 - A quel point Etes vous satisfait de votre avancement? ", Int64.Type}, {"Q6 - A quel point Etes vous satisfait du fait d'apprendre de nouvelles choses? ", Int64.Type}, {"Q7 -A quel point il a été difficile de vous orienter vers la Data?", type text}, {"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?", type text}, {"Q9 - Masculin/Féminin ?", type text}, {"Q10 - Age", Int64.Type}, {"Q11 - Dans quel pays habité vous?", type text}, {"Q12 -Niveau d'éducation ", type text}, {"Q13 - Ethnie", type text}}),
	    #"Split Column by Delimiter" = Table.SplitColumn(#"Changed Type", "Q1 - Position Actuel", Splitter.SplitTextByDelimiter(" (Please Specify)", QuoteStyle.Csv), {"Q1 - Position Actuel.1", "Q1 - Position Actuel.2"}),
	    #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Q1 - Position Actuel.1", type text}, {"Q1 - Position Actuel.2", type text}}),
	    #"Renamed Columns" = Table.RenameColumns(#"Changed Type1",{{"Q1 - Position Actuel.1", "Q1 - Position Actuel"}, {"Q1 - Position Actuel.2", "Q1 - Position Actuel (Précision)"}}),
	    #"Replaced Value" = Table.ReplaceValue(#"Renamed Columns",":","",Replacer.ReplaceText,{"Q1 - Position Actuel (Précision)"}),
	    #"Replaced Value1" = Table.ReplaceValue(#"Replaced Value","","Non déclaré",Replacer.ReplaceValue,{"Q1 - Position Actuel (Précision)"}),
	    #"Split Column by Delimiter1" = Table.SplitColumn(#"Replaced Value1", "Q4 - Industrie?", Splitter.SplitTextByDelimiter(" (Please Specify)", QuoteStyle.Csv), {"Q4 - Industrie?.1", "Q4 - Industrie?.2"}),
	    #"Changed Type2" = Table.TransformColumnTypes(#"Split Column by Delimiter1",{{"Q4 - Industrie?.1", type text}, {"Q4 - Industrie?.2", type text}}),
	    #"Renamed Columns1" = Table.RenameColumns(#"Changed Type2",{{"Q4 - Industrie?.1", "Q4 - Industrie?"}, {"Q4 - Industrie?.2", "Q4 - Industrie? Précision"}}),
	    #"Replaced Value2" = Table.ReplaceValue(#"Renamed Columns1",":","",Replacer.ReplaceText,{"Q4 - Industrie? Précision"}),
	    #"Replaced Value3" = Table.ReplaceValue(#"Replaced Value2","","Non déclaré",Replacer.ReplaceValue,{"Q4 - Industrie? Précision"}),
	    #"Split Column by Delimiter2" = Table.SplitColumn(#"Replaced Value3", "Q5 - Langage de Programmation préféré", Splitter.SplitTextByDelimiter(":", QuoteStyle.Csv), {"Q5 - Langage de Programmation préféré.1", "Q5 - Langage de Programmation préféré.2"}),
	    #"Changed Type3" = Table.TransformColumnTypes(#"Split Column by Delimiter2",{{"Q5 - Langage de Programmation préféré.1", type text}, {"Q5 - Langage de Programmation préféré.2", type text}}),
	    #"Renamed Columns2" = Table.RenameColumns(#"Changed Type3",{{"Q5 - Langage de Programmation préféré.1", "Q5 - Langage de Programmation préféré"}, {"Q5 - Langage de Programmation préféré.2", "Q5 - Langage de Programmation préféré (Précision)"}}),
	    #"Replaced Value5" = Table.ReplaceValue(#"Renamed Columns2","","Non déclaré",Replacer.ReplaceValue,{"Q5 - Langage de Programmation préféré (Précision)"}),
	    #"Split Column by Delimiter3" = Table.SplitColumn(#"Replaced Value5", "Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?", Splitter.SplitTextByDelimiter(" (Please Specify):", QuoteStyle.Csv), {"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?.1", "Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?.2"}),
	    #"Changed Type4" = Table.TransformColumnTypes(#"Split Column by Delimiter3",{{"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?.1", type text}, {"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?.2", type text}}),
	    #"Renamed Columns3" = Table.RenameColumns(#"Changed Type4",{{"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?.1", "Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?"}, {"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?.2", "Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité? (Précision)"}}),
	    #"Split Column by Delimiter4" = Table.SplitColumn(#"Renamed Columns3", "Q11 - Dans quel pays habité vous?", Splitter.SplitTextByDelimiter(" (Please Specify)", QuoteStyle.Csv), {"Q11 - Dans quel pays habité vous?.1", "Q11 - Dans quel pays habité vous?.2"}),
	    #"Changed Type5" = Table.TransformColumnTypes(#"Split Column by Delimiter4",{{"Q11 - Dans quel pays habité vous?.1", type text}, {"Q11 - Dans quel pays habité vous?.2", type text}}),
	    #"Renamed Columns4" = Table.RenameColumns(#"Changed Type5",{{"Q11 - Dans quel pays habité vous?.1", "Q11 - Dans quel pays habité vous?"}, {"Q11 - Dans quel pays habité vous?.2", "Q11 - Dans quel pays habité vous? (Précision)"}}),
	    #"Replaced Value6" = Table.ReplaceValue(#"Renamed Columns4",":","",Replacer.ReplaceText,{"Q11 - Dans quel pays habité vous? (Précision)"}),
	    #"Replaced Value7" = Table.ReplaceValue(#"Replaced Value6","","Non déclaré",Replacer.ReplaceValue,{"Q11 - Dans quel pays habité vous? (Précision)"}),
	    #"Split Column by Delimiter5" = Table.SplitColumn(#"Replaced Value7", "Q13 - Ethnie", Splitter.SplitTextByDelimiter(" (Please Specify):", QuoteStyle.Csv), {"Q13 - Ethnie.1", "Q13 - Ethnie.2"}),
	    #"Changed Type6" = Table.TransformColumnTypes(#"Split Column by Delimiter5",{{"Q13 - Ethnie.1", type text}, {"Q13 - Ethnie.2", type text}}),
	    #"Renamed Columns5" = Table.RenameColumns(#"Changed Type6",{{"Q13 - Ethnie.1", "Q13 - Ethnie"}, {"Q13 - Ethnie.2", "Q13 - Ethnie (Précision)"}}),
	    #"Replaced Value8" = Table.ReplaceValue(#"Renamed Columns5",":","",Replacer.ReplaceText,{"Q13 - Ethnie (Précision)"}),
	    #"Replaced Value9" = Table.ReplaceValue(#"Replaced Value8","","Non déclaré",Replacer.ReplaceValue,{"Q13 - Ethnie (Précision)"}),
	    #"Trimmed Text" = Table.TransformColumns(#"Replaced Value9",{{"Q1 - Position Actuel (Précision)", Text.Trim, type text}, {"Q4 - Industrie? Précision", Text.Trim, type text}, {"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité? (Précision)", Text.Trim, type text}, {"Q11 - Dans quel pays habité vous? (Précision)", Text.Trim, type text}, {"Q13 - Ethnie (Précision)", Text.Trim, type text}}),
	    #"Lowercased Text" = Table.TransformColumns(#"Trimmed Text",{{"Q1 - Position Actuel (Précision)", Text.Lower, type text}, {"Q4 - Industrie? Précision", Text.Lower, type text}, {"Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité? (Précision)", Text.Lower, type text}, {"Q11 - Dans quel pays habité vous? (Précision)", Text.Lower, type text}, {"Q13 - Ethnie (Précision)", Text.Lower, type text}}),
	    #"Duplicated Column" = Table.DuplicateColumn(#"Lowercased Text", "Q3 - Salaire actuel (en USD)", "Q3 - Salaire actuel (en USD) - Copy"),
	    #"Split Column by Character Transition" = Table.SplitColumn(#"Duplicated Column", "Q3 - Salaire actuel (en USD) - Copy", Splitter.SplitTextByCharacterTransition({"0".."9"}, (c) => not List.Contains({"0".."9"}, c)), {"Q3 - Salaire actuel (en USD) - Copy.1", "Q3 - Salaire actuel (en USD) - Copy.2", "Q3 - Salaire actuel (en USD) - Copy.3"}),
	    #"Split Column by Character Transition1" = Table.SplitColumn(#"Split Column by Character Transition", "Q3 - Salaire actuel (en USD) - Copy.2", Splitter.SplitTextByCharacterTransition((c) => not List.Contains({"0".."9"}, c), {"0".."9"}), {"Q3 - Salaire actuel (en USD) - Copy.2.1", "Q3 - Salaire actuel (en USD) - Copy.2.2"}),
	    #"Removed Columns" = Table.RemoveColumns(#"Split Column by Character Transition1",{"Q3 - Salaire actuel (en USD) - Copy.2.1", "Q3 - Salaire actuel (en USD) - Copy.3"}),
	    #"Changed Type7" = Table.TransformColumnTypes(#"Removed Columns",{{"Q3 - Salaire actuel (en USD) - Copy.1", Int64.Type}, {"Q3 - Salaire actuel (en USD) - Copy.2.2", Int64.Type}}),
	    #"Added Custom" = Table.AddColumn(#"Changed Type7", "Salaire Moyen", each List.Average({[#"Q3 - Salaire actuel (en USD) - Copy.1"] ,[#"Q3 - Salaire actuel (en USD) - Copy.2.2"]})),
	    #"Colonne multipliée" = Table.TransformColumns(#"Added Custom", {{"Salaire Moyen", each _ * 1000, type number}}),
	    #"Removed Columns1" = Table.RemoveColumns(#"Colonne multipliée",{"Q3 - Salaire actuel (en USD) - Copy.1", "Q3 - Salaire actuel (en USD) - Copy.2.2"}),
	    #"Removed Other Columns" = Table.SelectColumns(#"Removed Columns1",{"Identifiant", "Email", "Date d'interview (America/New_York)", "Heure  d'interview  (America/New_York)", "Durée de l'interview", "Q1 - Position Actuel", "Q2 - Avez changé de carrière?", "Q3 - Salaire actuel (en USD)", "Q4 - Industrie?", "Q5 - Langage de Programmation préféré", "Q6 - A quel point Etes vous satisfait de votre salaire? ", "Q6 - A quel point Etes vous satisfait de votre équilibre viepriéé/vie professionnel? ", "Q6 - A quel point Etes vous satisfait de vos collègues? ", "Q6 - A quel point Etes vous satisfait de votre hiérarchie? ", "Q6 - A quel point Etes vous satisfait de votre avancement? ", "Q6 - A quel point Etes vous satisfait du fait d'apprendre de nouvelles choses? ", "Q7 -A quel point il a été difficile de vous orienter vers la Data?", "Q8 - Si vous recherchiez un nouvel position, quel serait votre priorité?", "Q9 - Masculin/Féminin ?", "Q10 - Age", "Q11 - Dans quel pays habité vous?", "Q12 -Niveau d'éducation ", "Q13 - Ethnie", "Salaire Moyen"}),
	    #"Renamed Columns6" = Table.RenameColumns(#"Removed Other Columns",{{"Q6 - A quel point Etes vous satisfait de votre salaire? ", "Q6A - A quel point Etes vous satisfait de votre salaire?"}, {"Q6 - A quel point Etes vous satisfait de votre équilibre viepriéé/vie professionnel? ", "Q6B - A quel point Etes vous satisfait de votre équilibre viepriéé/vie professionnel?"}, {"Q6 - A quel point Etes vous satisfait de vos collègues? ", "Q6C - A quel point Etes vous satisfait de vos collègues?"}, {"Q6 - A quel point Etes vous satisfait de votre hiérarchie? ", "Q6D - A quel point Etes vous satisfait de votre hiérarchie?"}, {"Q6 - A quel point Etes vous satisfait de votre avancement? ", "Q6E - A quel point Etes vous satisfait de votre avancement?"}, {"Q6 - A quel point Etes vous satisfait du fait d'apprendre de nouvelles choses? ", "Q6F - A quel point Etes vous satisfait du fait d'apprendre de nouvelles choses?"}})
	in
	    #"Renamed Columns6".

A l'issue de ces transformations, les données sont chargées dans Power BI Desktop.

### Visualisation

Pour analyser les données, nous procèderons en deux étapes. En premier lieu, il faut examiner les caractéristiques de nos répondants en termes démographique ou de secteur d'activité. De cette manière, on pourrait apprécier par exemple, la représentativité de l'échantillon. En second lieu, nous pouvons faire ressortir les éléments saillants de l'enquête, en l'occurence, les préférences, les tendances et les défis auxquels les professionnels des données sont confrontés. Chacun de ces aspects sera afficher sur une page :

Page 1 : Caractéristiques de l'échantillon

- Une carte présentant le nombre d'enquêtés
- Une carte présentant l'age moyen des enquêtés
- Un Graphique en secteurs présentant la répartition des enquêtés par poste
- Un Treemap présentant la répartition des enquêtés par pays
- Un Graphique en anneau présentant la répartition des enquêtés par genre
- Un Graphique en anneau présentant la répartition des enquêtés par industrie


Page 2 : Principaux résultats

- Un Graphique en courbes et histogramme groupé présentant les Effectifs et salaires moyens par poste
- Une Matrice présentant le Niveau de satisfaction selon différents aspects et le poste occupé
- Un Graphique à barres empilés 100% présentant la Proportion des préférences de langage de prog. par poste
- Un Graphique à barres empilés 100% présentant l'Aspect prioritaire pour une nouvelle position selon le poste 



## Analyse et Principaux résultats

### Caractéristiques de l'échantillon


![Caractéristiques-des-enquêtés](/assets/img/Caractéristiques_des_enquêtés.gif)

Les informations affichées sur la première page ci-dessus, du rapport présente les caractéristiques de l'échantillon. On note que les Data Analyst représentent la majorité (près de 60%) des répondants. Leur âge moyen est de 30 ans et les 3/4 sont du genre masculin.

### Principaux enseignements et recommandations

L'analyse desdonnées des données nous révèle que les Data scientist sont les professionnals de la data les mieux payés en moyenne, suivis des Data Engineer et des Data Architect. Cela se remarque par rapport au niveau de la satisfaction concernant le salaire qui est plus élevé pour les Data scientist et les Data architect. Pour ces derniers, on remarque également que l'équilibre vie privée/vie professionnelle est la moins satisfaisante comparativement aux autres professionnels. En témoigne la proportion élevé (60%) de ces professionnels (Data architect) qui considèrent cet équilibre comme l'aspect prioritaire qui va conditionné le choix de leur prochaine position.
Par ailleurs, le langage de programmation préféré de professionnels est très largement python, et ceci, peut importe le poste considéré. En effet, la préférence pour ce langage va d'un minimum de 61% (chez les autres professionnels de la donnée) à un maximu de 100% (chez les Data Architect).

![Principaux-résultats](/assets/img/Principaux___résultats.png)

Ce projet a permis de mettre en lumière que le salaire, l’équilibre vie privée/vie professionnelle et la possibilité de télétravail sont les facteurs qui influence les professionnel de la donnée dans leur choix de carrière. De plus, il montre que le langage Python est langage de programmation préféré de ces professionnels quel que soit leur spécialité. 

En conclusion, les firmes recrutant des professionnels de la donnée pourrait rendre leur offre plus intéressante, non seulement grâce à un bon salaire, mais surtout en mettant l'accent sur l'équilibre vie privée/ vie professionnelle et la possibilité de travailler à distance. Aussi, offrir des solutions utilisant comme langage de programmation python permettrait de toucher plus majoritairement les professionnel de la donnée.
 
