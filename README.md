# project1
# Recommender System using Pandas

Author: Artsem Harachau

## Uruchomienie projektu na własnym komputerze

1. Zainstaluj [Anaconda](https://www.anaconda.com/products/individual) oraz Python


2. (Opcjonalnie) Można też zainstalować IDE [PyCharm](https://www.jetbrains.com/pycharm/) (community version)


3. Zainstaluj [Git](https://git-scm.com/download/win)


4. Następnie zrób Fork projektu do swojego Github. Wykorzystaj [dokumentację](https://docs.github.com/en/get-started/quickstart/fork-a-repo)


5. Stworzenie środowiska conda:
        
        1. Otwórz terminal Anaconda Prompt

        2. W terminale przejdź do głównego folderu projektu. W razie potrzeby warto zwracać się do [conda user-guide](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file):
          
           (stworzenie środowiska z nazwą rs-class-env) conda env create --name rs-class-env -f environment.yml


6. Aktywacja środowiska conda:
        
        1. W terminalu Anaconda Prompt przejdź do głównego folderu projektu i uruchom następne polecenie:
            
           (aktywacja środowiska rs-class-env) conda activate rs-class-env


7. Następnie po aktywacji środowiska wpisz polecenie(również w Anaconda Prompt):
        
        jupyter notebook
	
        Wynik polecenia pokaże się w domyślnej przeglądarce.

8. W otwartym Jupyter Notebook otwórz project_2_recommender_and_evaluation.ipynb


9. Żeby uruchomić blok kodu w Jupyter Notebook można użyć kombinacji ctrl+enter


10. Ewentualne problemy w momencie pracy z projektem:


        1. Zamknięcie wszystkich stron Jupyter Notebook
        
           Rozwiązanie: w Anaconda Prompt użyj ctrl+c(czasami trzeba użyć tego skrótu 2-3 razy)        
        
        2. Zbyt długie uruchomienia bloku kodu(uruchomienie kodu w Jupyter Notebook oznaczone jest gwiazdką)
           
           Rozwiązanie: w Anaconda Prompt zamknij wszystkie strony Jupyter Notebook, a następnie uruchom Jupyter Notebook
     
        3. Error "ImportError: No module named..."
           
           Rozwiązanie: w Anaconda Prompt zamknij wszystkie strony Jupyter Notebook, następnie użyj polecenia pip install module_named i po instalacji uruchom Jupyter Notebook


## Przejście przez ważne momenty kodu

        1. W funkcji prepare_users_df stworzyłem kod dla dwóch przypadków features: one-hot encoding oraz prawdopodobieństwo wartości

        2. W funkcji prepare_items_df stworzyłem kod dla dwóch przypadków features: one-hot encoding oraz features oparte na minimalnych oraz maksymalnych wartościach numerycznych kolumn w dataframe

        3. Żeby sprawdzić te metody w rekomenderze wystarczy odkomentować odpowiedni kod w tych funkcjach i zakomentować kod innej metody

        Uwaga:
              1. W przypadku testowania users features opartego na prawdopodobieństwach wartości w metodzie fit rekomendera trzeba odkomentować linijkę kodu `interactions_df = interactions_df.fillna(0).astype(int)`.
                 W innych przypadkach ona nie jest potrzebna.

              2. W przypadku testowania rekomendera na modelu SVRCBUIRecommender trzeba odkomentować w metodzie fit linijkę `interactions_df = interactions_df.sample(frac=0.05)`.
                 Ta linijka weźmie tylko 5% danych z dataframe. Ja sprawdzałem na podziałach danych 70%, 50%, 30%, która żadna(oprócz 5%) tak i nie uruchomiła tuningu. Domyślam się
                 że po godzinach, kiedy przerywałem proces dane ciągle się ładowały do modelu. W przypadku z 5% danych oni też ładowały się kilka godzin, chociaż ostatecznie tuning zaczął działać.



## Testowanie rekomendera na różnych modelach i różnych kombinacji features

Tuning Modeli:

1. LinearRegressionCBUIRecommender, LassoRegressionCBUIRecommender - tuning zawsze uruchamiałem na max_evals=10

2. LassoCVRegressionCBUIRecommender, RandomForestCBUIRecommender, SVRCBUIRecommender - tuning na max_evals=100

3. XGBoostCBUIRecommender - tuning na max_evals=300


Uwaga: tuning modeli zwłaszcza na max_evals > 10, zajmuje dosyć dużo czasu i zauważyłem, że zależy od method robienia features oraz wydajnego kodu w generowaniu negative_features w metodzie fit rekomendera.

        
Wyniki tuningu w porównaniu z AmazonRecommender:

1. User features - one-hot encoding, items features - min-max values
	
![Image text](https://github.com/ArtsemHarachau/project1/blob/master/project1/tuning_screens/combine_images_one-hot_min-max.png?raw=true)

2. User features - one-hot encoding, items features - one-hot encoding

![Image text](https://github.com/ArtsemHarachau/project1/blob/master/project1/tuning_screens/combine_images_one-hot_one-hot.png?raw=true)

3. User features - probability of values, items features - min-max values

![Image text](https://github.com/ArtsemHarachau/project1/blob/master/project1/tuning_screens/combine_images_prob_min-max.png?raw=true)

4. User features - probability of values, items features - one-hot encoding

![Image text](https://github.com/ArtsemHarachau/project1/blob/master/project1/tuning_screens/combine_images_prob_one-hot.png?raw=true)



