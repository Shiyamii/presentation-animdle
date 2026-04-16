Animedle — Description & Architecture

C'est quoi

Jeu type Wordle pour anime. Joueur devine un anime (ou personnage) via indices — attributs comparés révèlent si guess correct ou proche.
                  
---                                                                                                                                                                                                                    
Modes de jeu

┌───────────────────┬────────────────────┬───────────────────────────────┐
│       Mode        │       Route        │          Description          │
├───────────────────┼────────────────────┼───────────────────────────────┤
│ Daily             │ /daily             │ Anime du jour, même pour tous │
├───────────────────┼────────────────────┼───────────────────────────────┤
│ Endless           │ /endless           │ Animes en chaîne, sans limite │                                                                                                                                             
├───────────────────┼────────────────────┼───────────────────────────────┤                                                                                                                                             
│ Character         │ /character         │ Devine un personnage          │                                                                                                                                             
├───────────────────┼────────────────────┼───────────────────────────────┤                                                                                                                                             
│ Character Endless │ /character-endless │ Personnages en chaîne         │
├───────────────────┼────────────────────┼───────────────────────────────┤                                                                                                                                             
│ Challenge         │ /challenge         │ Mode multijoueur en salle     │
└───────────────────┴────────────────────┴───────────────────────────────┘
                  
---                                                                                                                                                                                                                    
Stack

┌─────────────┬──────────────────────────────────────────────────────────────────┐
│   Couche    │                              Techno                              │
├─────────────┼──────────────────────────────────────────────────────────────────┤
│ Frontend    │ React 19, Vite, Tailwind v4, React Router v7, Zustand, shadcn/ui │
├─────────────┼──────────────────────────────────────────────────────────────────┤
│ Backend     │ Hono, Bun, MongoDB                                               │                                                                                                                                     
├─────────────┼──────────────────────────────────────────────────────────────────┤                                                                                                                                     
│ Auth        │ better-auth (email/password)                                     │                                                                                                                                     
├─────────────┼──────────────────────────────────────────────────────────────────┤                                                                                                                                     
│ Realtime    │ WebSocket (rooms multijoueur)                                    │
├─────────────┼──────────────────────────────────────────────────────────────────┤
│ Déploiement │ Docker + Nginx                                                   │
└─────────────┴──────────────────────────────────────────────────────────────────┘
   
---                                                                                                                                                                                                                    
Architecture Frontend (MVVM)

src/
├── pages/
│   ├── home/          HomePageView + useHomePageViewModel                                                                                                                                                             
│   ├── daily/         DailyGuessingPage                                                                                                                                                                               
│   ├── endless/       EndlessModePage + ViewModel                                                                                                                                                                     
│   ├── character/     CharacterGuessingPage + CharacterEndlessPage                                                                                                                                                    
│   ├── challenge/     ChallengePage (multijoueur)
│   ├── admin/         AdminPage (tabs: Daily, Animes, Characters, Stats)                                                                                                                                              
│   ├── auth/          AuthPage + ViewModel                                                                                                                                                                            
│   └── account/       AccountPage                                                                                                                                                                                     
├── components/                                                                                                                                                                                                        
│   ├── ui/            shadcn/ui base components
│   ├── character/     CharacterGuessRows, HintSpoilerBlock, etc.                                                                                                                                                      
│   ├── GuessTable     Tableau des tentatives                                                                                                                                                                          
│   └── AutoComplete   Recherche d'anime/perso                                                                                                                                                                         
├── stores/            Zustand (animeStore, userStore, challengeStore, characterDifficultyStore)                                                                                                                       
└── lib/               auth-client, ws-client, guessing-utils
                                                                                                                                                                                                                         
---                                                                                                                                                                                                                    
Architecture Backend

backend/src/
├── routes/
│   ├── anime.ts        GET animes, guess, current daily
│   ├── auth.ts         Login/register (better-auth)                                                                                                                                                                   
│   ├── admin.ts        CRUD animes & persos, stats
│   ├── room.ts         Salles multijoueur (create/join)                                                                                                                                                               
│   └── room-guess.ts   Guesses dans une salle                                                                                                                                                                         
├── services/                                                                                                                                                                                                          
│   ├── AnimeService    Logique daily + endless (singleton)                                                                                                                                                            
│   ├── CharacterService Logique perso (singleton)                                                                                                                                                                     
│   └── RoomService     Logique salle multijoueur
├── repositories/                                                                                                                                                                                                      
│   ├── AnimeRepository / CharacterRepository
│   └── CurrentAnimeRepository / CurrentCharacterRepository                                                                                                                                                            
├── wsHandlers.ts       WebSocket pour challenge live                                                                                                                                                                  
└── index.ts            Hono app + cron daily (minuit UTC)
Cron : chaque nuit minuit UTC → updateGoalAnime() + updateGoalCharacter() pour changer le défi du jour.                                                                                                                
