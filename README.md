# coorelation-with-python

import pandas as pd
import numpy as np
import seaborn as sns

import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
import matplotlib
plt.style.use('ggplot')
from matplotlib.pyplot import figure

%matplotlib inline
# Adjusting the configuration of the plots
matplotlib.rcParams['figure.figsize'] = (12,8)

pd.options.mode.chained_assignment = None

# Importing the data
df = pd.read_csv(r'movies.csv')

# Looking at the data
df.head()

	name 	rating 	genre 	year 	released 	score 	votes 	director 	writer 	star 	country 	budget 	gross 	company 	runtime
0 	The Shining 	R 	Drama 	1980 	June 13, 1980 (United States) 	8.4 	927000.0 	Stanley Kubrick 	Stephen King 	Jack Nicholson 	United Kingdom 	19000000.0 	46998772.0 	Warner Bros. 	146.0
1 	The Blue Lagoon 	R 	Adventure 	1980 	July 2, 1980 (United States) 	5.8 	65000.0 	Randal Kleiser 	Henry De Vere Stacpoole 	Brooke Shields 	United States 	4500000.0 	58853106.0 	Columbia Pictures 	104.0
2 	Star Wars: Episode V - The Empire Strikes Back 	PG 	Action 	1980 	June 20, 1980 (United States) 	8.7 	1200000.0 	Irvin Kershner 	Leigh Brackett 	Mark Hamill 	United States 	18000000.0 	538375067.0 	Lucasfilm 	124.0
3 	Airplane! 	PG 	Comedy 	1980 	July 2, 1980 (United States) 	7.7 	221000.0 	Jim Abrahams 	Jim Abrahams 	Robert Hays 	United States 	3500000.0 	83453539.0 	Paramount Pictures 	88.0
4 	Caddyshack 	R 	Comedy 	1980 	July 25, 1980 (United States) 	7.3 	108000.0 	Harold Ramis 	Brian Doyle-Murray 	Chevy Chase 	United States 	6000000.0 	39846344.0 	Orion Pictures 	98.0

# Finding a percentage of null values
for col in df.columns:
    pct_missing = np.mean(df[col].isnull())
    print('{} - {}%'.format(col, round(pct_missing*100)))

name - 0%
rating - 1%
genre - 0%
year - 0%
released - 0%
score - 0%
votes - 0%
director - 0%
writer - 0%
star - 0%
country - 0%
budget - 28%
gross - 2%
company - 0%
runtime - 0%

# Droping the rows with null values
df = df.dropna(how='any',axis=0)

# Checking data types

df.dtypes

name         object
rating       object
genre        object
year          int64
released     object
score       float64
votes       float64
director     object
writer       object
star         object
country      object
budget      float64
gross       float64
company      object
runtime     float64
dtype: object

# Changing the data type of the budget amd gross columns
# from float to integer
df['budget'] = df['budget'].astype('int64')
df['gross'] = df['gross'].astype('int64')

df.head()

	name 	rating 	genre 	year 	released 	score 	votes 	director 	writer 	star 	country 	budget 	gross 	company 	runtime
0 	The Shining 	R 	Drama 	1980 	June 13, 1980 (United States) 	8.4 	927000.0 	Stanley Kubrick 	Stephen King 	Jack Nicholson 	United Kingdom 	19000000 	46998772 	Warner Bros. 	146.0
1 	The Blue Lagoon 	R 	Adventure 	1980 	July 2, 1980 (United States) 	5.8 	65000.0 	Randal Kleiser 	Henry De Vere Stacpoole 	Brooke Shields 	United States 	4500000 	58853106 	Columbia Pictures 	104.0
2 	Star Wars: Episode V - The Empire Strikes Back 	PG 	Action 	1980 	June 20, 1980 (United States) 	8.7 	1200000.0 	Irvin Kershner 	Leigh Brackett 	Mark Hamill 	United States 	18000000 	538375067 	Lucasfilm 	124.0
3 	Airplane! 	PG 	Comedy 	1980 	July 2, 1980 (United States) 	7.7 	221000.0 	Jim Abrahams 	Jim Abrahams 	Robert Hays 	United States 	3500000 	83453539 	Paramount Pictures 	88.0
4 	Caddyshack 	R 	Comedy 	1980 	July 25, 1980 (United States) 	7.3 	108000.0 	Harold Ramis 	Brian Doyle-Murray 	Chevy Chase 	United States 	6000000 	39846344 	Orion Pictures 	98.0

# Creating the correct year column

df['yearcorrect'] = df['released'].astype(str).str.split(', ').str[-1].astype(str).str[:4]

df.head()

	name 	rating 	genre 	year 	released 	score 	votes 	director 	writer 	star 	country 	budget 	gross 	company 	runtime 	yearcorrect
0 	The Shining 	R 	Drama 	1980 	June 13, 1980 (United States) 	8.4 	927000.0 	Stanley Kubrick 	Stephen King 	Jack Nicholson 	United Kingdom 	19000000 	46998772 	Warner Bros. 	146.0 	1980
1 	The Blue Lagoon 	R 	Adventure 	1980 	July 2, 1980 (United States) 	5.8 	65000.0 	Randal Kleiser 	Henry De Vere Stacpoole 	Brooke Shields 	United States 	4500000 	58853106 	Columbia Pictures 	104.0 	1980
2 	Star Wars: Episode V - The Empire Strikes Back 	PG 	Action 	1980 	June 20, 1980 (United States) 	8.7 	1200000.0 	Irvin Kershner 	Leigh Brackett 	Mark Hamill 	United States 	18000000 	538375067 	Lucasfilm 	124.0 	1980
3 	Airplane! 	PG 	Comedy 	1980 	July 2, 1980 (United States) 	7.7 	221000.0 	Jim Abrahams 	Jim Abrahams 	Robert Hays 	United States 	3500000 	83453539 	Paramount Pictures 	88.0 	1980
4 	Caddyshack 	R 	Comedy 	1980 	July 25, 1980 (United States) 	7.3 	108000.0 	Harold Ramis 	Brian Doyle-Murray 	Chevy Chase 	United States 	6000000 	39846344 	Orion Pictures 	98.0 	1980

# Changing the option to be able to scroll through the data

pd.set_option('display.max_rows', None)

# Finding and droping duplicate values

df['company'].drop_duplicates().sort_values(ascending=False)

7129                                              thefyzz
5664                                          micro_scope
4007                                             i5 Films
6793                                           i am OTHER
6420                                                 erbp
3776                                       double A Films
3330                          Zucker Brothers Productions
146                                      Zoetrope Studios
2213                                   Zeta Entertainment
3698                              Zentropa Entertainments
1180                                 Zenith Entertainment
5180                                      Zazen Produções
1321                             Zanuck/Brown Productions
1329                          Zacharias-Buhai Productions
789                             Young Sung Production Co.
5125                           Young Hannibal Productions
5499                                          Yellow Bird
4618                                       Yash Raj Films
4990                            Yari Film Group Releasing
5410                                Yari Film Group (YFG)
5583                                X-Filme Creative Pool
6265                              Worldview Entertainment
4392                          World of Wonder Productions
4999                  World Wrestling Entertainment (WWE)
425                                   World Film Services
4581                                  Working Title Films
4272                                         Wiseau-Films
450                                   Winwood Productions
3943                                        Winkler Films
2084                                        WingNut Films
2355                                 Wildwood Enterprises
6606                                       Wildgaze Films
5276                                           Wild Bunch
4863                      Wiedemann & Berg Filmproduktion
5550                                  Why Not Productions
4572                                     Whitewater Films
6616                               WhiteFlame Productions
1306                                           White Lair
1475                                          White Eagle
1744                                       Westerly Films
1304                        Weintraub Entertainment Group
5496                                Wayfare Entertainment
6672                                        Waverly Films
4505                    Warner Independent Pictures (WIP)
7267                                Warner Bros. Pictures
2284                    Warner Bros. Family Entertainment
6578                    Warner Bros. Digital Distribution
2341                               Warner Bros. Animation
0                                            Warner Bros.
7401                               Warner Animation Group
75                                Walt Disney Productions
688                                  Walt Disney Pictures
117                         Walt Disney Animation Studios
5075                                         Walden Media
4975                                                  WIP
7420                                          Votiv Films
5272                                     Voltage Pictures
5409                                Vivendi Entertainment
1115                                   Vista Organization
6647                                    Visiona Romantica
1307                                           Vision PDG
3539                            Village Roadshow Pictures
2467                               View Askew Productions
1894                        Victor Company of Japan (JVC)
1716                                       Victor & Grais
4964                              Victoires International
1522                                     Vestron Pictures
5302                                        Vertigo Films
6540                                       Verisimilitude
7018                                    Verdi Productions
7263                                Vendian Entertainment
2504                                       Vegahom Europe
4827                         Vanguard Films and Animation
251                                       VAE Productions
6208                                    Upside Down Films
7594                                      Unplanned Movie
5635               Universal Pictures International (UPI)
6                                      Universal Pictures
2943                               Universal City Studios
350                                     Unity Productions
227               United Film Distribution Company (UFDC)
3017                              United Artists Pictures
4190                      United Artists Film Corporation
9                                          United Artists
6460                                         Unison Films
2490                             Ultra Muchos Productions
3883                                            USA Films
4785                                      UK Film Council
6396                                        Two Ton Films
5640                                     Two Prong Lesson
4694                                     Twisted Pictures
4691                      Twentieth Century Fox Animation
28                                  Twentieth Century Fox
2122                                  Turner Pictures (I)
6352                                         Trésor Films
6280                             Truth Entertainment (II)
4626                                      True West Films
2769                                        Triumph Films
2305                                     Trimark Pictures
3577                                  Tribeca Productions
2280                                   TriStar Television
503                                      TriStar Pictures
6552                                   Treehouse Pictures
6252                                             Translux
1290                          Trancas International Films
6973                                Toy Fight Productions
2081                         Touchwood Pacific Partners 1
501                                   Touchstone Pictures
6093                                         Total Recall
5491                                       Tornasol Films
3097                                 Too Askew Prod. Inc.
6473                                    Tonik Productions
4110                                          Tonic Films
2349                                         Tomson Films
3356                                         Tomboy Films
3849                                        Tokuma Shoten
1264                          Tokuma Japan Communications
3589                                        Toho Pictures
773                                          Toho Company
7432                                         Toei Company
2862                               Tim Burton Productions
1649                                      Tig Productions
388                             Tiberius Film Productions
3820                              Three Rivers Production
849                        Thorn EMI Screen Entertainment
4312                             This Is That Productions
1499                                   The Zanuck Company
4773                                The Weinstein Company
7579                              The Tyler Perry Company
858                               The Saul Zaentz Company
1179                           The Samuel Goldwyn Company
6390                                   The Safran Company
2827                                The Rank Organisation
1494                                      The Movie Group
1278                                    The Mount Company
2450                              The Mark Gordon Company
10                                    The Malpaso Company
5835                             The Last Picture Company
139                                      The Ladd Company
6379                                   The Lab of Madness
2924                            The Kushner-Locke Company
7344                                      The Ink Factory
5474                                  The Halcyon Company
1144                             The Guber-Peters Company
257                                    The Geffen Company
4733                                     The Family Stone
537                                      The Cannon Group
5824                            The American Film Company
5518                                     Ternion Pictures
5470                            Temple Hill Entertainment
599                                    Tartan Productions
2774                                       Tapestry Films
356                              TaliaFilm II Productions
4804                                    Tag Entertainment
7459                                    TSG Entertainment
1537                                    TMS Entertainment
4935                                            THINKFilm
1063                          TAFT Entertainment Pictures
7057                                              Syncopy
6285                                    Sycamore Pictures
2753                                      Sweetland Films
5005                               Sweet Tea Pictures LLC
358                                 Sunn Classic Pictures
904                                    Sunbow Productions
7033                                      Summit Premiere
4263                                 Summit Entertainment
2729                                    Suburban Pictures
2074                                          StudioCanal
2626                                         Studio Trite
5308                                        Studio Ghibli
7356                                             Studio 8
5953                                            Studio 37
1845                        Strong Heart/Demme Production
4479                                 Strike Entertainment
4789                                     Stratus Film Co.
6593                            Stoney Lake Entertainment
1883                                 Stone Group Pictures
2366                              Steve White Productions
6359                                  Steel Mill Pictures
3954                                        Star Overseas
5584                                        Stage 6 Films
5837                          Stacey Testro International
3583                               Spyglass Entertainment
495                                      Spinal Tap Prod.
7277                                        SpectreVision
4280                        Spectacle Entertainment Group
6841                                   Sparkle Roll Media
1340                                Spacegate Productions
6350                                  Space Rocket Nation
4181                               South Pacific Pictures
6761                    Sony Pictures Entertainment (SPE)
4435                               Sony Pictures Classics
6740                              Sony Pictures Animation
5114                                         Solana Films
6259                                          SnowPiercer
5925                                  Snoot Entertainment
4059                                          Slough Pond
5997                                     Skyscraper Films
620                                              Skyewiay
7474                                       Skydance Media
1978                                   Siriol Productions
4172                            Silver Sphere Corporation
1697                            Silver Screen Partners IV
1417                           Silver Screen Partners III
6791                                          Silver Reel
1444                                      Silver Pictures
6439                                 Sikhya Entertainment
5130                          Sidney Kimmel Entertainment
4252                                            Show East
3405                                     Shooting Gallery
5330                                      Shocking Bottle
7368                                    ShivHans Pictures
3238                                Sheinberg Productions
6897                                     SharpSword Films
1313                    Shapiro-Glickenhaus Entertainment
4617                             Shangri-La Entertainment
3397                          ShadowCatcher Entertainment
449                                  Seventh Avenue Films
3742                                  Seven Arts Pictures
4793                              Serendipity Point Films
2757                                       Seraphim Films
5398                             Senator Entertainment Co
4346                                         SenArt Films
5998                                         Semtex Films
6727                                        See-Saw Films
5772                                  Sedic International
1580                             Sean S. Cunningham Films
374                                         Seagoat Films
2545                             Seagal/Nasso Productions
3529                                          Screen Gems
6464                                     Screen Australia
2946                              Scott Rudin Productions
6544                               Scott Free Productions
7178                                          Scion Films
3322                                              Sciarlò
2503                                       Savoy Pictures
3760                                         Saturn Films
2720                                Sandollar Productions
276                              Sandcastle 5 Productions
3096                                Samuelson Productions
5110                                        Samuels Media
4737                                 Samuel Goldwyn Films
3202                         Samson Productions Pty. Ltd.
7216                                       Salon Pictures
7132                                          Sailor Bear
3834                                  Saban Entertainment
7264                                            STX Films
6938                                    STX Entertainment
6484                                    SModcast Pictures
646                                  SLM Production Group
5965                                      SBS Productions
2829                                 Rysher Entertainment
4182                                      Rumbalara Films
6625                                           Ruby Films
6763                              Route One Entertainment
6064                                           Roth Films
4754                                 Room 9 Entertainment
6620                                           Rook Films
4522                                       Rogue Pictures
5511                                                Rogue
3213                           Robert Simonds Productions
6389                                 Roadside Attractions
2399                           Road Movies Filmproduktion
502                                River Road Productions
5736                             River Road Entertainment
847                                         Rimfire Films
5393                                        Rifkin-Eberts
6306                           Riddick Canada Productions
277                      Richmond Light Horse Productions
2388                         Richard Williams Productions
3349                                        Rhombus Media
3855                                   Revolution Studios
3705                            Revelations Entertainment
1673                                           Reteitalia
5967                                       Reprisal Films
4690                                 Rent Productions LLC
1513                                     Renn Productions
96                                   Renaissance Pictures
1509                                    Renaissance Films
909                                   Religioso Primitiva
5167                                     Relativity Media
6535                             Reel FX Creative Studios
3089                                        Redwave Films
4891                                       Red Hour Films
6243                                 Red Granite Pictures
6236                                Red Crown Productions
1733                       Recorded Picture Company (RPC)
5767                                      Realitism Films
6604                            RatPac-Dune Entertainment
2413                                   Rastar Productions
15                                        Rastar Pictures
426                                          Rastar Films
247                               Rankin/Bass Productions
5233                                     Rainforest Films
6773                                        Raindog Films
2336                  Raffaella De Laurentiis Productions
230                                          RKO Pictures
6580                                           RADiUS-TWC
4046                                     R.P. Productions
4106                                Quinta Communications
7001                                     Quad Productions
3113                                        Q Productions
6899                                            Pyramania
6600                                Pure Flix Productions
3764                                 Punch 21 Productions
5914                                   Pt. Merantau Films
3832                                    Prufrock Pictures
5656                                 Protagonist Pictures
6566                                    Prospero Pictures
2358                                     Propaganda Films
1288                                   Prominent Features
861                    Producers Sales Organization (PSO)
1441                Producers Representative Organization
4828                           Prime Film Productions LLC
7114                                     Primate Pictures
2251                                  Price Entertainment
6405                                           Prettybird
774                                  Pressman Productions
5017                                        Pressman Film
5990                                    Preferred Content
6376                                        Prana Studios
3775                          Portman Entertainment Group
6607                                     Porchlight Films
2811                                    Populist Pictures
2956                                     Pope Productions
4538                                  Pop Pop Productions
5412                                    Ponty Up Pictures
94                                      Polygram Pictures
2324                        Polygram Filmed Entertainment
372                                Polyc International BV
192                                     PolyGram Pictures
36                          PolyGram Filmed Entertainment
3388                                  Polar Entertainment
7332                                  Point Grey Pictures
4304                                  Plunge Pictures LLC
5371                                             Playtone
2785                     Playhouse International Pictures
3619                                         Play It Inc.
2603                                   Planet Productions
6067                                 Plan B Entertainment
3260                              Pixar Animation Studios
4179                                          Pioneer LDC
5340                                         Picturehouse
2387                                   Picture Securities
3098                                     Phoenix Pictures
445                       Phillips Whitehouse Productions
3008                                 Peters Entertainment
2420                                 Permut Presentations
7149                               Perfect World Pictures
2214                                       Penta Pictures
2216                                          Penta Films
2221                                  Penta Entertainment
5382                          Pelican Productions LLC (I)
7111                                  Paulilu Productions
5605                               Pathé Renn Productions
3517                         Pathé Pictures International
1688                                  Pathé Entertainment
6386                                                Pathé
2808                                    Party Productions
6651                                      Parts and Labor
21                                   Partisan Productions
6658                                          Participant
2378                                  Parkway Productions
2086                            Parkes/Lasker productions
5043                                    Paramount Vantage
7476                                    Paramount Players
3                                      Paramount Pictures
4920                                   Paramount Classics
6751                                  Paramount Animation
970                            Paragon Arts International
3187          Papazian-Hirsch Entertainment International
7206                                      Pantelion Films
6962                                       Panorama Films
2712                               Pandora Filmproduktion
3850                                       Pandora Cinema
5482                                       Pan Européenne
240                                              Pan Arts
6659                                       PalmStar Media
1323                                                Palla
1320                                      Palace Pictures
1828                                   Paisley Park Films
1815                                      Pacific Western
3010                                    PVM Entertainment
5190                                       P2 Productions
5357                                       Overture Films
5690                                      Overnight Films
6072                              Overbrook Entertainment
1457                               Outlaw Productions (I)
4979                     Out of the Blue... Entertainment
412                           Osterman Weekend Associates
5620                                         Oscilloscope
4                                          Orion Pictures
6771                                      Origin Pictures
3353                       Oriental Light and Magic (OLM)
5935                                 Open Road Films (II)
6415                                      Open Road Films
3752                                      Open City Films
6813                                           Onyx Films
190                                   One Way Productions
5942                                        Omnilab Media
5795                                  Omega Entertainment
5791                                     Olympus Pictures
1835                                              Odyssey
4193                                          Odeon Films
3021                                        October Films
5434                               Occupant Entertainment
4786                           OLC / Rights Entertainment
3148                                      O Entertainment
5358                                       Number 9 Films
1763                             Nova International Films
6467                                Northern Lights Films
2134                        Northern Lights Entertainment
1508                             Norman Twain Productions
6339                                     No Trace Camping
4998                                   No Matter Pictures
1398                            No Frills Film Production
4591                           Nine Yards Two Productions
3285                              Nimbus Film Productions
5158                               Night and Day Pictures
703                                       Night Life Inc.
1709                                Nicolas Entertainment
3318                                      Next Wave Films
4668                                   Next Entertainment
3631                              Newmarket Capital Group
526                                    New World Pictures
1236                        New World Entertainment Films
1416                                          New Visions
1346                          New Sky Communications Inc.
2703                              New Regency Productions
3773                                   New Oz Productions
483                                       New Line Cinema
2414                                 New Horizons Picture
867                 New Century Entertainment Corporation
3184                          New Amsterdam Entertainment
2555                            Nest Family Entertainment
405                                               Nelvana
1861                                 Nelson Entertainment
335                                      National Lampoon
1045                                               Natant
6010                                     NVSH Productions
3795                                    NPV Entertainment
3007                                      NFH Productions
2904                                    NDF International
1291                                      NBC Productions
3777                              Myung Film Company Ltd.
3271                                 Mystery Clock Cinema
4093                                      Myriad Pictures
3543                                  Mutual Film Company
4127                                     Muse Productions
2975                             Mundy Lane Entertainment
2604                               Mulholland Productions
57                            Mulberry Square Productions
4606                                             Movision
6814                                               Motlys
2977         Motion Picture Corporation of America (MPCA)
5311                                               Mosaic
4199    Morra, Brezner, Steinberg and Tenenbaum Entert...
1265                           Morgan Creek Entertainment
7302                                           Mooz Films
6210                              Monsterfoot Productions
7469                                Monkeypaw Productions
3401                                     Monarch Pictures
5534                                     Mod Producciones
2432                                              Miramax
681                                    Mirage Enterprises
7526                                Mimran Schur Pictures
7491                                     Millennium Media
5029                                     Millennium Films
389                                             Millenium
4778                                      Milkshake Films
6145                                         Milk & Media
3290                           Midwinter Productions Inc.
3920                              Metropolitan Filmexport
120                             Metro-Goldwyn-Mayer (MGM)
3031                           Merchant Ivory Productions
4391                                         Mepris Films
6425                             Memento Films Production
42                               Melvin Simon Productions
3055                              Melampo Cinematografica
3598                                          Medusa Film
6053                                      Mediaset España
5823                                             Mediapro
7320                           Media Rights Capital (MRC)
4119                                     Media Asia Films
4267                                Media 8 Entertainment
3090                                      McFarlane Films
6804                                      Mayhem Pictures
6360                                Max Films Productions
5269                                   Matten Productions
4225                                             Material
7363                                    Matchbox Pictures
6043                                       Marvel Studios
6448                                 Marvel Entertainment
4269                                   Marvel Enterprises
5606                                           Marv Films
3640                                 Marked Entertainment
6471                                          Marcy Media
106                                 Mann/Caan Productions
3538                                     Mandeville Films
5200                                     Mandate Pictures
5982                                      Mandalay Vision
4400                                    Mandalay Pictures
3071                               Mandalay Entertainment
1553                                  Malpaso Productions
891                                   Maljack Productions
114                                 Major Studio Partners
2230                         Majestic Films International
1503                                 Magnum Pictures Inc.
5574                                    Magnolia Pictures
5775                                 Magic Light Pictures
2083                             Mace Neufeld Productions
7169                                     Macari/Edelstein
1292                             Mac and Me Joint Venture
6783                                          MWM Studios
3466                                            MTV Films
609                                    MPL Communications
5593                                      MPI Media Group
6138                                      MK2 Productions
3731                         MFF Feature Film Productions
2953                                        MDP Worldwide
3757                                        Légende Films
4289                               Lynda Obst Productions
7423                                           Lyla Films
6570                                         Lutzus-Brown
2599                                     Lumière Pictures
2660                                     Lumiere Pictures
6224                                Lucky Monkey Pictures
368                              Lucille Ball Productions
5366                                  Lucasfilm Animation
2                                               Lucasfilm
7305                                  Lucamar Productions
4004                                Lost Soul Productions
7151                        Los Angeles Media Fund (LAMF)
286                                   Lorimar Productions
1155                              Lorimar Motion Pictures
20                             Lorimar Film Entertainment
3042                               Longview Entertainment
5559                                    Lleju Productions
3901                                           LivePlanet
2043                                   Live Entertainment
1902                             Little Tokyo Productions
6745                                      Little Stranger
4925                                            Lionsgate
3348                                     Lions Gate Films
7336                                         Likely Story
1543                              Lightyear Entertainment
2699                             Lightstorm Entertainment
1721                                   Lightning Pictures
5411                              LightWave Entertainment
3732                          Lewitt / Eberts Productions
1711                                   Levins-Henenlotter
6159                                 Les Films du Losange
3847                                Les Films Alain Sarde
1989                                         Les Films 21
4375                                        Les Armateurs
226                            Leisure Investment Company
6487                              Legendary Entertainment
6936                                       Legendary East
7361                                             Legend3D
652                             Legend Production Company
879                           Lawrence Gordon Productions
6855                                      Lava Bear Films
1314                                           Laurenfilm
1727                            Laurence Mark Productions
5000                             Last Holiday Productions
2082                             Largo International N.V.
1844                                  Largo Entertainment
2742                                       Lancaster Gate
3700                              Lakeshore Entertainment
5442                                            Lago Film
371                                              Ladbroke
6120                                      La Petite Reine
6633                                        LStar Capital
5519                                     LD Entertainment
7260                                      LBI Productions
877                                            L.A. Films
3126                                    Kuzui Enterprises
1904                                             Krisjair
2433                                  Krasnow Productions
6500                                Kramer & Sigman Films
2606                             Kouf/Bigelow Productions
2877                               Kopelson Entertainment
3940                                      Konrad Pictures
3419                                   Knock Films A.V.V.
4427                                         Klasky-Csupo
497                                  Kingsmere Properties
4122                                      Kingsgate Films
929                              Kings Road Entertainment
4580                                      Kim Ki-Duk Film
3934                                         Killer Films
7430                             Kevin Downes Productions
101                            Kennedy Miller Productions
6907                                       Keep Your Head
2052                                      Kasdan Pictures
5107                           Karbo Vantas Entertainment
3129                                Kanun parvaresh fekri
4630                            Kang Je-Kyu Film Co. Ltd.
6934                                     K5 International
3253                                             Juno Pix
4830                                        Junebug Movie
5683                              Josephson Entertainment
1897                      Joseph S. Vecchio Entertainment
2539                            Jones Entertainment Group
5576                             Jon Shestack Productions
6479                                            Jolie Pas
4719                               John Wells Productions
3573                                  Jim Henson Pictures
4523                                  Jet Tone Production
2481                                         Jersey Films
3663                              Jerry Bruckheimer Films
870                                Jay Weston Productions
3186                             Jasmine Productions Inc.
295                                     James Glickenhaus
549                                         Jaffe-Lansing
1698                          Jackson/McHenry Company,The
50            Jack Rollins & Charles H. Joffe Productions
3480                          Jack Giarraputo Productions
1520                           JVC Entertainment Networks
3216                                      JLT Productions
390                                        JF Productions
2737                                      JDI Productions
1018                                    J&M Entertainment
1465                                               Ixtlan
3222                                Ivory Way Productions
5586                             It's a Laugh Productions
2102                                         Island World
917                                       Island Pictures
1747                                       Ironbark Films
6081                        Iron Horse Entertainment (II)
6200                                    Intrepid Pictures
1769                            Interscope Communications
220                          International Film Investors
130                International Cinema Corporation (ICC)
56                                   International Cinema
3304                                     Intermedia Films
5240                                           Intermedia
6237                                     Insurge Pictures
167                                           Innovisions
890                                      Initial Pictures
2029                    Initial Entertainment Group (IEG)
5504                      Infinity Features Entertainment
6052                                    Indian Paintbrush
2985                            Independent Pictures (II)
1164                              Independent Film Centre
219                 Incorporated Television Company (ITC)
5676                       Incentive Filmed Entertainment
2242                               In The Bag Productions
1691                   Imperial Entertainment Corporation
7136                             Imperative Entertainment
1163                          Imagine Films Entertainment
1380                                Imagine Entertainment
5595                              Imagi Animation Studios
2064                                   Image Organization
2681                                         Image Comics
6688                           Illumination Entertainment
7537                                           Iconoclast
4415                                     Icon Productions
2330                     Icon Entertainment International
1955                                            IRS Media
17                                              IPC Films
3936    IMF Internationale Medien und Film GmbH & Co. ...
6139                                            IM Global
4796                                      IFC Productions
4820                                            IFC Films
6628                                            IAC Films
3379                                    Hyperion Pictures
5846                                             Hydraulx
5633                              Hyde Park Entertainment
6772                                     Hurwitz Creative
5010                                   Hunting Lane Films
7297                            Hunter Killer Productions
6918                                   Human Stew Factory
1647                                 Hughes Entertainment
7343                              Huayi Brothers Pictures
7220                                 Huayi Brothers Media
7303                                       Huayi Brothers
7156                             Houston King Productions
5502                                House Row Productions
6588                                   Hopscotch Features
5041                                 Hoot Productions LLC
1143                                Home Box Office (HBO)
1671                                   Hollywood Pictures
3884                            Hollywood Licensing Group
1983                                             Hivemind
1585                                        Hill/Rosenman
3172                              High School Sweethearts
1597                                 High Bar Productions
6911                      Hidden Empire Film Group (HEFG)
7616                               Hicktown Entertainment
396                                   Hickmar Productions
6877                                         Heyday Films
5821                                Herrick Entertainment
182                                     Heron Productions
1112                                 Heron Communications
5802                                   Hero Entertainment
307                                            Herb Jaffe
848                                Henson Associates (HA)
580                                               Hemdale
2273                                 Hell's Kitchen Films
400                                              Heat GBR
6205                                    Headline Pictures
3459                                          Haxan Films
595                          Hawn / Sylbert Movie Company
2694                                                Havoc
5617                                        Haut et Court
7307                                               Hasbro
3291                                    Harvest Filmworks
7399                                 Hartbeat Productions
3744                             Hart Sharp Entertainment
3398                                          Harpo Films
427                  Harold Robbins International Company
3484                            Happy Madison Productions
2381                            Hanna-Barbera Productions
4097                              Handprint Entertainment
108                                        HandMade Films
5170                                         HanWay Films
3750                               Hammerhead Productions
2719                             Halloween VI Productions
499                                     Hal Roach Studios
5112                                       Haishang Films
3762                                   Haft Entertainment
3331                                            HSX Films
3438                                  HK Film Corporation
5193                                          HDNet Films
584                                          HBO Pictures
4236                                            HBO Films
1185                                         H.I.T. Films
2653                                        Guys Upstairs
129                                  Gurian Entertainment
3761                                     Gullane Pictures
6468                                  Gulfstream Pictures
3048                                                Guild
5156                              Groundswell Productions
7092                               Grisbi Productions, Le
4019                                 Greisman Productions
665                            Greenwich Film Productions
2530                                Greenleaf Productions
3793                                          Green/Renzi
2886                                         Green Parrot
6094                                      Green Hat Films
6958                                  Greater Productions
1048             Great American Films Limited Partnership
4309                                     Gray Daisy Films
6317                                  Gravier Productions
4527                               Grand Slam Productions
2246                                Gramercy Pictures (I)
6926                                         Gracie Films
6702                                         Gotham Group
1507                                       Gordon Company
4907                                         Googly Films
5342                                Goodspeed Productions
6297                                        Good Universe
2930                                         Good Machine
2209                                Golden Way Films Ltd.
105                                Golden Harvest Company
229                         Goldcrest Films International
7540                                              GoldDay
4080                                    Gold Circle Films
3061                                    Golar Productions
254                              Golan-Globus Productions
7294                                     Goddard Textiles
7314                                    Goalpost Pictures
4431                                             Go Films
5820                                        Glass Eye Pix
942                                 Gladden Entertainment
3461                               Ghoulardi Film Company
6154                                 Ghost House Pictures
110                           Georgetown Productions Inc.
6925                                         George Films
6556                                          Genre Films
1870                                      Geffen Pictures
2125                                              Gaumont
6196                             Gary Sanchez Productions
6592                                Gail Katz Productions
5548                                             GK Films
765                                                   GGG
5480                                               G-BASE
4117                                Fuzzy Bunny Films (I)
4399                                      Funny Boy Films
142                                           Fulvia Film
3974                                         Fuller Films
2346                                    Full Crew/Say Yea
7026                                          Fuego Films
4892                                             Fu Works
1296                                          Front Films
7274                                 Frenesy Film Company
6792                                        Freedom Media
918                            Freddie Fields Productions
3465                                   Franchise Pictures
3015                             Fox Searchlight Pictures
586                              Fox Run Productions Inc.
4962                                           Fox Atomic
2891                                    Fox 2000 Pictures
1149                                      Fourth Protocol
6581                             Four Seasons Partnership
4458                                         Forward Pass
5260                                    FortyFour Studios
6827                                         Fortis Films
3281                                                Forge
6401            Forest Whitaker's Significant Productions
6185                                  Foresight Unlimited
7034                                    Forecast Pictures
6349                           Follow Through Productions
1944                                        Fogwood Films
4174                                    Focus Puller Inc.
4171                                       Focus Features
3623                                   Flying Heart Films
6922                                     Flashlight Films
4126                                   Flan de Coco Films
2711                             First Look International
4162                               First Light Production
7084                                Fingerprint Releasing
2425                                   Fine Line Features
14                                      Filmways Pictures
1898                                             Films A2
353                                Filmplan International
534                                  Filmline Productions
5404                                 Filmax International
4487                                         Filmax Group
5642                                               Filmax
1493                                             Filmauro
783                                  Filmation Associates
7331                             FilmNation Entertainment
3990                                             FilmFour
5657                                         FilmDistrict
3440                                           FilmColony
6255                                                Film4
2394                                        Film Workshop
5035                                         Film Science
4986                           Film Partner International
2274                                      Film Andes S.A.
6294                                              Film 44
3661                                        Figment Films
2107                           Fifth Avenue Entertainment
5401                                    Field Guide Films
5483                                       Fidélité Films
2867                                              Fiction
6512                                 Fewlas Entertainment
7010                                        Fermion Films
6576                                    Fear of God Films
3256                                Fear and Loathing LLC
2816                             Farabi Cinema Foundation
2570                                         Fallingcloud
6323                             Faliro House Productions
6840                                      FaithStep Films
4769                                        Fairway Films
6905                               Fairview Entertainment
5523                            Fade to Black Productions
3418                                        Fade In Films
3790                                             FX Sound
4161                                      FTM Productions
2755                                      FIT Productions
3288                                    FGM Entertainment
2098                                            FAI Films
1054                                                  F/M
6111                                Exclusive Media Group
5716                                      Exclusive Films
4456                              Evolution Entertainment
4896                                    Everyman Pictures
3639                                       Evenstar Films
4090                                           EuropaCorp
3878                                      Eureka Pictures
778                                   Eurasia Investments
4614                               Eulogy Productions LLC
5618                                         Euforia Film
2546                                          Etalon Film
4851                                     Estudios Picasso
650                       Estudios Churubusco Azteca S.A.
2026                                 Esperanza Films Inc.
1361                                              Esparza
6700                                       Escape Artists
986                                 Escalante Productions
6037                                       Eros Worldwide
4203                              Epsilon Motion Pictures
1738                                     Epic Productions
112                                       Eon Productions
5952                               Enticing Entertainment
7522                Entertainment Studios Motion Pictures
6764                                    Entertainment One
4699                  Entertainment Manufacturing Company
118                                    Enigma Productions
453                                 Englander Productions
4497                                Endgame Entertainment
6147                                Enderby Entertainment
2089                               Encino Man Productions
2167                               Enchantment Films Inc.
3412                              Enchanter Entertainment
1108                                    Emshell Producers
691                                       Empire Pictures
7604                                       Emphatic Films
4389                       Emperor Multimedia Group (EMG)
6197                 Emmett/Furla/Oasis Films (EFO Films)
127                                      Embassy Pictures
224                        Embassy International Pictures
3600                                       Elysian Dreams
5984                                          Elle Driver
2952                           Eleventh Street Production
6656                                     Element Pictures
4989                               Electric Entertainment
6857                          Electric City Entertainment
7088                                       Elara Pictures
3953                                             El Deseo
4825                                   El Camino Pictures
4429                                           Eikon Film
2519                                         Egg Pictures
280                                      Efer Productions
4102                                           Edko Films
517                                 Edge City Productions
604                          Edgar J. Scherick Associates
4666                                      Eden Rock Media
1474                             Eddie Murphy Productions
4041                                         Ecosse Films
6207                                    Eclectic Pictures
5535                              Echo Lake Entertainment
176                                     Eaves Movie Ranch
384                                     Eagle Associates.
3518                                           ETIC Films
4507                                                 ERBP
195                                             EMI Films
5296                                                 EFTI
6671                         Duplass Brothers Productions
5800                               Dune Entertainment III
6438                                   Dune Entertainment
3106                                  Dreamworks Pictures
3278                                 DreamWorks Animation
4883                                           DreamWorks
8                                          Dovemead Films
2880                               Doug Liman Productions
438                                           Double Play
7159                          Double Nickel Entertainment
6140                                 Double Feature Films
6645                                Double Dare You (DDY)
2328                                   Dorset Productions
2469                     Donner/Shuler-Donner Productions
589                   Don Simpson/Jerry Bruckheimer Films
2529                                            Don Bluth
3302                                             Dollface
3564                                        Dogstar Films
6242                                     Dog Run Pictures
2768                                    Dog Eat Dog Films
7208                             Disruption Entertainment
6636                                   Disneytoon Studios
3824                          Disney Television Animation
2158                    Dino de Laurentiis Communications
113                            Dino De Laurentiis Company
2846                                      Dimension Films
2879                             Digital Image Associates
6403                                Dickhouse Productions
7218                                          Diagonal TV
3263                                      DiNovi Pictures
4764                              Di Bonaventura Pictures
3989                                   Dharma Productions
1725                                  Devoted Productions
1684                                Detour Filmproduction
3594                                    Destination Films
2211                                   Desperate Pictures
741                             Desert Hearts Productions
6780                                       Depth of Field
6859                                               Dentsu
954                                  Delphi V Productions
658                                Delphi III Productions
432                                          Delphi Films
5234                                        Deerjen Films
4620                               Deep River Productions
7451                                        Decibel Films
168                           DeHaven/Shapiro Productions
5604                                     De Line Pictures
410               De Laurentiis Entertainment Group (DEG)
3151                                        De Fina-Cappa
7101                                         Dayday Films
3203                                             Daybreak
1574                                         Davros Films
1899                             Davis-Panzer Productions
2318                                          Davis-Films
1662                                  Davis Entertainment
3199                          David Kirschner Productions
779                              David Foster Productions
3553                              David Brown Productions
3492                            Dark Castle Entertainment
6050                                  Danmarks Radio (DR)
1472                                               Danjaq
4052                                            DNA Films
6015                                             DJ Films
3411                                    DIC Entertainment
2171                       DENTSU Music And Entertainment
5019                                      DEJ Productions
3149                                     DC Entertainment
444                                     D & P Productions
5551                                     Curmudgeon Films
782                                Crystalite Productions
3559                                Crystal Sky Worldwide
4788                                       Crunk Pictures
3389                            Cruise/Wagner Productions
3722                                  Cruella Productions
2456                                      Crowvision Inc.
3495                                     Crossroads Films
6562                                 Cross Creek Pictures
1257                                        Cristaldifilm
1432                                        Crescent Moon
4822                              Crescent Drive Pictures
7098                                             Cre Film
3609                               Craven-Maddalena Films
3621                              Cradle Productions Inc.
6807                                         Covert Media
5907                                  Cottonwood Pictures
3905                              Cort/Madden Productions
5969                                               Corsan
1324                                Cornelius Productions
3681                            Copperheart Entertainment
4357                                          ContentFilm
2688                          Constellation Entertainment
2702                                        Constellation
6100                        Constantin Film International
487                                       Constantin Film
2151    Consejo Nacional para la Cultura y las Artes (...
1194                                   Conquering Unicorn
1717                                    Concorde Pictures
3670                                      Compulsion Inc.
5827                                        Company Films
1205                        Commies From Mars Corporation
1100                         Columbia Pictures Industries
4516               Columbia Pictures Film Production Asia
1                                       Columbia Pictures
5702                                          Color Force
536                              Colgems Productions Ltd.
5437                                    Cohen Media Group
6575                              Codeblack Entertainment
3736                                   Code Entertainment
6238                                        Cocktail Film
4424                                   Cobalt Media Group
4697                                         Coach Carter
7645                              Clubhouse Pictures (II)
3771                                   Cloud Ten Pictures
7157                                    Cloud Eight Films
6058                              Cloud Atlas Productions
5689                      Closest to the Hole Productions
3655                                        Clipsal Films
3197                                     Clinica Estetico
5945                            Cliffjack Motion Pictures
2872                                         Clavius Base
3860                           Claudie Ossard Productions
402                                            City Films
1052                                         Circle Films
3305                        Cinépix Film Properties (CFP)
2614                                            Cineville
1619                                        Cinetel Films
530                               Cinesthesia Productions
2493                       Cinergi Pictures Entertainment
3789                      Cinerenta Medienbeteiligungs KG
6187                                            Cinereach
718                                               Cinepro
1212                                 Cineplex Odeon Films
231                                      Cinematograph AB
1800                             Cinemarque Entertainment
5628                                             CinemaNX
3084                         Cinema Line Film Corporation
187                                 Cinema Group Ventures
1317                                    Cinema City Films
100                                            Cinema '84
6587                                        Cinelou Films
1712                                             Cinehaus
1486                                         Cinecorp SAC
1146                          Cinecom Entertainment Group
4231                                             CineWild
5616                                CineSon Entertainment
1604                               Cine Location Services
2269                                            CiBy 2000
2195                     Christopher Columbus Productions
7455                             Chris Morgan Productions
3965                                Chris Lee Productions
6337                                  Chockstone Pictures
2342                          Chiodo Brothers Productions
3426                              Chinese Bookie Pictures
6437                  China Film Group Corporation (CFGC)
3189                 China Film Co-Production Corporation
3994                                      Chickie the Cop
4148                                 Cheyenne Enterprises
7229                           Chestnut Ridge Productions
3745                                   Chester Films Inc.
4134                                     Cherry Sky Films
3633                             Cherry Alley Productions
6507                                Chernin Entertainment
3493                                      Cheerleader LLC
6212                                          Chase Films
7                            Chartoff-Winkler Productions
99                         Charles H. Schneer Productions
467                           Charles Burnett Productions
1783                                   Channel Four Films
2576                                        Channel Films
2121                           Challenge Film Corporation
263                                      Chai Productions
3606                                            Centurion
3273                         Centropolis Film Productions
4771                                Cent Productions Inc.
5077                                     Celluloid Dreams
375                                       Celandine Films
5266                                        Celador Films
2645                                 Cecchi Gori Pictures
1401              Cecchi Gori Group Tiger Cinematografica
2400                              Cecchi Gori Europa N.V.
6759                                               Caviar
7269                                              Cave 76
33                               Cattle Annie Productions
4299                            Casual Friday Productions
1458                            Castle Rock Entertainment
4213                                Castelao Producciones
5301                             Casey Silver Productions
5956                                         Carver Films
930                               Carthago Films S.a.r.l.
5138                                Carsey-Werner Company
5858                            Carousel Productions (II)
1145                                     Carolco Pictures
1065                           Carolco International N.V.
1504                                Carolco Entertainment
1976                           Carnival Film & Television
2477                                     Caravan Pictures
5208                                Captivity Productions
569                                             Capricorn
3040                                     Cappa Production
5116                                        Capitol Films
932                             Capital Equipment Leasing
1936                                Capella International
2372                                        Capella Films
2462                                 Capcom Entertainment
1130                                  Canon Italia S.r.l.
815                                       Cannon Pictures
1268                                 Cannon International
950                                          Cannon Group
406                                     Cannon Films Inc.
393                                          Cannon Films
1305                                 Cannon Entertainment
2045                                       Candyman Films
7557                                         Canana Films
4578                                        Canal+ España
3546                           Canal+ Droits Audiovisuels
2094                                               Canal+
1566                  Canadian International Studios VIII
37                                              Camp Hill
4518                                     Camelot Pictures
228                                 Cambridge Productions
6375                                   Callahan Filmworks
3298                                 Calimari Productions
7457                                             CalMaple
312                            Cable and Wireless Finance
398                                            Cabal Film
5215                                     CTB Film Company
2319                                              CNCAIMC
4128                                     CJ Entertainment
134                               CIP Filmproduktion GmbH
7015                                            CG Cinéma
611                                  CBS Theatrical Films
5722                                            CBS Films
5011                           C.O.R.E. Feature Animation
512                                  C.H.U.D. Productions
11                                           C.A.T. Films
4265                                         C-2 Pictures
305                                C & C Brown Production
5154                             Bórd Scannán na hÉireann
3192                                  Butcher's Run Films
6923                                       Burk A Project
3917                                 Bungalow Productions
6603                                   Bullwinkle Studios
4900                             Bull's Eye Entertainment
4836                                         Bulbul Films
7477                            Buena Vista International
4462                       Buena Vista Home Entertainment
795                                Bud Yorkin Productions
2550                                        Buckeye Films
22                                      Bryna Productions
2962                                  Bruin Grip Services
6048                          Brownstone Productions (II)
7095                        Brownstone Entertainment (II)
2794                        Brothers McMullen Productions
126                                           Brooksfilms
1968                               Bronze Eye Productions
5638                                   Bronco Productions
6529                              Broken Road Productions
4576                             Broken Lizard Industries
5369                                       Broadway Video
3327                                    Broadway Pictures
6638                                 Broad Green Pictures
1215                           British Screen Productions
3362    British Broadcasting Corporation (BBC) Television
2149               British Broadcasting Corporation (BBC)
3825                        Brillstein-Grey Entertainment
1785                                        Brigand Films
5415                             Bridgit Folman Film Gang
363                                 Breathless Associates
7070                                         Braven Films
3751                              Branti Film Productions
3768                                          Brant-Allen
3072                               Brandywine Productions
80                               Borough Park Productions
7014                                      Bona Film Group
3473                                Bona Fide Productions
4342                 Boll Kino Beteiligungs GmbH & Co. KG
5028                                           Bold Films
7262                          Boies / Schiller Film Group
4477                                 Bob Yari Productions
6501                                       Bob Industries
6710                                Blumhouse Productions
5934                                   Blueprint Pictures
6312                                      Bluegrass Films
6230                                    Blue Yonder Films
3086                               Blue Tulip Productions
4157                               Blue Train Productions
3421                                    Blue Streak Films
5339                                     Blue Sky Studios
6943                                       Blue Sky Films
1318                                  Blue Rider Pictures
7511                            Blue Budgie Films Limited
5259                                           Blue Askew
4033                                     Blow Up Pictures
6808                                                Bloom
4187                                    Blockbuster Films
5182                                     Block 2 Pictures
6158                                       Block / Hanson
6679                               Blinding Edge Pictures
6119                               Bleiberg Entertainment
6733                                Bleecker Street Films
5590                        Blank of the Dead Productions
6639                                           BlackWhite
6726                                    Black Label Media
6565                 Black Entertainment Television (BET)
7054                          Black Bicycle Entertainment
6461                                  Black Bear Pictures
5639                                 Bill Kenwright Films
1854                                    Bill Graham Films
3613                                 Bigel / Mailer Films
7566                                 Big Talk Productions
6493                               Big Screen Productions
4640                              Big Red Dog Productions
6834                                   Big Indie Pictures
3354                                        Big Dog Films
5418                                    Big City Pictures
6003                                      Big Beach Films
4070                                      Beverly Detroit
5438                              Berk/Lane Entertainment
4714                           Bergman Lustig Productions
4468                                          BenderSpink
2589                            Ben-Ami/Leeds Productions
6299                                      Bellanova Films
4821                               Belladonna Productions
3703                                Bel Air Entertainment
6520                   Beijing Skywheel Entertainment Co.
6023                             Beijing New Picture Film
7652                  Beijing Diqi Yinxiang Entertainment
7565    Beijing Dengfeng International Culture Communi...
5915                             Before The Door Pictures
1506                                           Beco Films
2700                           Beckner/Gorman Productions
1884                                Beacon Communications
2845                                        Bazmark Films
4605                                  Bazelevs Production
566                                          Bavaria Film
3578                     Baumgarten-Prophet Entertainment
3218                                  Bates Entertainment
5390                                         Barunson E&A
133                           Barry & Enright Productions
115                Barclays Mercantile Industrial Finance
2624                                      Barcelona Films
3791                                 Banner Entertainment
3210                               Bandeira Entertainment
4029                                Bandai Visual Company
1960                                   Baltimore Pictures
3558                            Baldwin/Cohen Productions
740                                 Balcor Film Investors
2066                                     Bakshi Animation
5710                                            Bad Robot
2096                                  Bad Lt. Productions
6132                     Bad Cop Bad Cop Film Productions
2951                                 Bad Bird Productions
504                            Bachelor Party Productions
5387                                              Babylon
6585                                 Baby Way Productions
6895                                     BZ Entertainment
6687                                         BRON Studios
6826                                             BN Films
4748                                         BET Pictures
7439                                            BET Films
6201                                        BCDF Pictures
6480                           BBL Motion Picture Studios
594                                                   BBE
2825                                            BBC Films
6665                                                  B24
7123                                              B Story
3530                                  Azoff Entertainment
7143                                    Awesomeness Films
3359                                  Award Entertainment
3714                                         Avrora Media
2055                             Avnet/Kerner Productions
7333                                      Aviron Pictures
7254                                 Avi Arad Productions
5168                                Avi Arad & Associates
3706                                            Avery Pix
1500                                      Avenue Pictures
5625                                     Autonomous Films
6563                              Automatik Entertainment
2932           Australian Film Finance Corporation (AFFC)
7564                                Aurora Alliance Films
223                                                Aurora
2371                                 August Entertainment
3387    Audiovisual Development Bureau, Ministerio da ...
5705                                 Audiovisual Aval SGR
2571                                             Audifilm
6822                                          Audax Films
7285                                       Atomic Monster
6027                                    Atlas Productions
3311                                  Atlas Entertainment
7588                                     Atlantic Records
6157                               Atlantic Pictures (II)
351                          Atlantic Entertainment Group
2189                                          Atchafalaya
3510                             Asymmetrical Productions
6801                                         Astute Films
3474                                          Astralwerks
97                                  Astral Bellevue Pathé
6935                                       Astrakan Films
2847                                        Astoria Films
258                                    Aspen Film Society
3675                 Asia Union Film & Entertainment Ltd.
5903                           Asghar Farhadi Productions
2143                                           Ascot Film
5194                                   Ascendant Pictures
3596                              Arts Council of England
5970                                Artists Public Domain
6984                                        Artists First
509                                      Artistry Limited
141                                    Artista Management
3423                                Artisan Entertainment
2364                                               Artimm
3755                                Artic Productions LLC
5213                                        Artfire Films
7154                                              Artbees
1479                               Art Linson Productions
1914                          Arnold Kopelson Productions
7360                                         Armory Films
6742                                      Arka Mediaworks
379                       Aries Cinematográfica Argentina
4970                                   ArieScope Pictures
1933                                          Argos Films
1961                                                Arena
4306                            Archer Street Productions
7377                                          Archer Gray
529                                          Arcafin B.V.
6171                                      Arcade Pictures
5031                                      Arc Productions
5838                            Aramid Entertainment Fund
1101                                       Applied Action
6298                                           Appian Way
3866                                              Apostle
4212                             ApolloMedia Distribution
5120                                   Apatow Productions
7348                                      Apartment Story
5225                                    Anonymous Content
2615                                              Annhall
5840                                Annapurna Productions
6258                                   Annapurna Pictures
3804                               Animal Productions LLC
7119                                         Animal Logic
7139                                       Animal Kingdom
4290                                          Angry Films
2405                                  Ang Lee Productions
6355                            Andrew Lauren Productions
4701                          Andrea Sperling Productions
5911                                     Anchor Bay Films
590                          Anarchist's Convention Films
210                                         Anabasis N.V.
2048                                    American Zoetrope
1627                  American Playhouse Theatrical Films
714                                    American Playhouse
977                                    American Filmworks
1246              American Entertainment Partners II L.P.
3268                          American Empirical Pictures
58                            American Cinema Productions
1064                                       Amercent Films
3252                                        Amen Ra Films
3575                            Ambridge Film Partnership
355                                  Amblin Entertainment
6893                                       Amazon Studios
6730                                 Amasia Entertainment
3644                                Am Psycho Productions
4015                              Alter Ego Entertainment
3690                                      Altavista Films
6180                               Also Known As Pictures
4186                                     Alphaville Films
639                                            Almost You
237                                          Almena Films
6824                                    Allspark Pictures
7131                                  Alloy Entertainment
6662                         Allison Shearmur Productions
1525                                        Allied Vision
2758                                    Allied Stars Ltd.
2070                                    Allied Filmmakers
6381                                       Alliance Films
2856                  Alliance Communications Corporation
3233                     Alliance Atlantis Communications
6149                                             Alliance
1529                                  Alleged Productions
4803                              Allan Zeman Productions
3224                                  All Net Productions
1284                                 All Girl Productions
1089                                          Alive Films
7385                                     Alibaba Pictures
6172                                         Alfama Films
6459                               Aldamisa Entertainment
2128                                          Alcor Films
3615                                  Alcon Entertainment
2625                                                  Aim
6271                                      Aggregate Films
3212                                               Agenda
6442                                     After Dark Films
7577                                         Affirm Films
2119                                   Adventure Pictures
2922                              Addis Wechsler Pictures
392                              Adams Apple Film Company
6016                                            Ada Films
6234                                     AdScott Pictures
1043                               Act III Communications
7290                                 Access Entertainment
7079                          Acacia Filmed Entertainment
1384                                  Abramoff Production
6091                                  Abraham Productions
6370                                 Abbolita Productions
5324                                Abandon Entertainment
1129                           Aaron Spelling Productions
1625                            Aaron Russo Entertainment
3667                                   Aardman Animations
6218                               Aamir Khan Productions
18                                  AVCO Embassy Pictures
6584                                     ASIG Productions
6314                                             AR Films
484                                                  AMLF
1615                                  AM/PM Entertainment
4802                                     AITD Productions
7006                                              AI-Film
1781                                            ADC Films
367                                   ABC Motion Pictures
6452                                                  A24
544                                             A&M Films
2819                                         A Band Apart
4146                                   98 MPH Productions
4350                                            900 Films
1655                                      888 Productions
1690                                       88 Productions
3829                                       7 Films Cinéma
3603                                 7 Arts International
5161                                  4Kids Entertainment
913                           40 Acres & A Mule Filmworks
3592                                 4 Kids Entertainment
4038                                  3Mark Entertainment
5199                                         360 Pictures
7211                                     3311 Productions
3579                       3 Miles Apart Productions Ltd.
2903                                 3 Arts Entertainment
7005                                                2DUX²
5218                                     2929 Productions
5597                                             26 Films
4118                                25th Hour Productions
2584                                   21st Century Films
1700                        21st Century Film Corporation
6528                                21 Laps Entertainment
7651                                 20th Century Studios
4559                                     2003 Productions
5195                                         2.4.7. Films
7489                                    2.0 Entertainment
4151                                         2 Loop Films
6517                     1984 Private Defense Contractors
7109                                           1978 Films
4412                                     19 Entertainment
385                                      1818 Productions
2929                                        1492 Pictures
3024                                      .406 Production
7525                  "Weathering With You" Film Partners
4345                      "DIA" Productions GmbH & Co. KG
Name: company, dtype: object

# Creating a scatter plot for budget and gross revenue

plt.scatter(x=df['budget'], y=df['gross'])
plt.title('Film Budget Vs. Gross Revenue')
plt.xlabel('Gross Revenue')
plt.ylabel('Film Budget')
plt.show()

# Plotting budget and gross revenue using seaborn to show a regression line

sns.regplot(x='budget', y='gross', data=df, scatter_kws={"color":"grey"}, line_kws={"color":"blue"})

<AxesSubplot:xlabel='budget', ylabel='gross'>

# Finding correlations

df.corr()

	year 	score 	votes 	budget 	gross 	runtime
year 	1.000000 	0.056386 	0.206021 	0.327722 	0.274321 	0.075077
score 	0.056386 	1.000000 	0.474256 	0.072001 	0.222556 	0.414068
votes 	0.206021 	0.474256 	1.000000 	0.439675 	0.614751 	0.352303
budget 	0.327722 	0.072001 	0.439675 	1.000000 	0.740247 	0.318695
gross 	0.274321 	0.222556 	0.614751 	0.740247 	1.000000 	0.275796
runtime 	0.075077 	0.414068 	0.352303 	0.318695 	0.275796 	1.000000

# We can see the highest correlation is between Film Budget and Gross Revenue
# We can easily identify them by making a correlation matrix
# The light color represents the higher correlations

correlation_matrix = df.corr()
sns.heatmap(correlation_matrix, annot=True)
plt.title('Correlation Matrix')
plt.xlabel('Movie variables')
plt.ylabel('Movie variables')
plt.show()

# Using unstack to view the correlations listed for each variable

corr_pairs = correlation_matrix.unstack()
print(corr_pairs)

year     year       1.000000
         score      0.056386
         votes      0.206021
         budget     0.327722
         gross      0.274321
         runtime    0.075077
score    year       0.056386
         score      1.000000
         votes      0.474256
         budget     0.072001
         gross      0.222556
         runtime    0.414068
votes    year       0.206021
         score      0.474256
         votes      1.000000
         budget     0.439675
         gross      0.614751
         runtime    0.352303
budget   year       0.327722
         score      0.072001
         votes      0.439675
         budget     1.000000
         gross      0.740247
         runtime    0.318695
gross    year       0.274321
         score      0.222556
         votes      0.614751
         budget     0.740247
         gross      1.000000
         runtime    0.275796
runtime  year       0.075077
         score      0.414068
         votes      0.352303
         budget     0.318695
         gross      0.275796
         runtime    1.000000
dtype: float64

# Sorting the correlation values

sorted_pairs = corr_pairs.sort_values(kind="quicksort")
print(sorted_pairs)

year     score      0.056386
score    year       0.056386
budget   score      0.072001
score    budget     0.072001
year     runtime    0.075077
runtime  year       0.075077
year     votes      0.206021
votes    year       0.206021
gross    score      0.222556
score    gross      0.222556
year     gross      0.274321
gross    year       0.274321
         runtime    0.275796
runtime  gross      0.275796
budget   runtime    0.318695
runtime  budget     0.318695
year     budget     0.327722
budget   year       0.327722
votes    runtime    0.352303
runtime  votes      0.352303
         score      0.414068
score    runtime    0.414068
votes    budget     0.439675
budget   votes      0.439675
votes    score      0.474256
score    votes      0.474256
gross    votes      0.614751
votes    gross      0.614751
budget   gross      0.740247
gross    budget     0.740247
year     year       1.000000
budget   budget     1.000000
votes    votes      1.000000
score    score      1.000000
gross    gross      1.000000
runtime  runtime    1.000000
dtype: float64

# Filtering the values with a correlation higher than 50%

strong_pairs = sorted_pairs[abs(sorted_pairs) > 0.5]
print(strong_pairs)

gross    votes      0.614751
votes    gross      0.614751
budget   gross      0.740247
gross    budget     0.740247
year     year       1.000000
budget   budget     1.000000
votes    votes      1.000000
score    score      1.000000
gross    gross      1.000000
runtime  runtime    1.000000
dtype: float64

We can conclude that the variables wiht highest correlation with Gross Revenue are Votes with 61.47% and Film Budget with 74.02%.
0
