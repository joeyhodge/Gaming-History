# Frostbite Engine - Technical Notes

 * [Frostbite on wikipedia][1]
 * These resources are available on [dice.se][2], [ea.com/frostbite][3] and [slideshare.net][4]

![](images/2020_11_03_frostbite_technical_notes/dragon-age.png)

## "Move to One Engine"

EA is moving its games, including Mass Effect and FIFA, [onto a single graphics engine][5]. Frostbite has evolved to become the cornerstone of these titles:

 * `FIFA` (FIFA 17/18)
 * `Mass Effect` (Mass Effect: Andromeda)
 * `Battlefield` (Battlefield 1 - 2016)  
 * `Need for Speed` (NEED FOR SPEED 2017: PAYING IT BACK)
 * `Mirror's Edge` (Mirror's Edge Catalyst - 2016)
 * `Star Wars` (Star Wars Battlefront) 
 * `Dragon Age` (Dragon Age: Origins)


## Resources

### Engine & Pipeline 

Year | Conf       | Title                                                                                           | Note
---- | ---------- | ----------------------------------------------------------------------------------------------- | ----
2017 | gdc2017    | [FrameGraph: Extensible Rendering Architecture in Frostbite][16]                                |
2016 | gdc2016    | [Optimize the graphics pipeline with compute][15]                                               |
2015 | siggraph15 | [The Rendering Pipeline - Challenges & Next Steps][14]                                          |
2013 | stockholm  | [Scaling the Pipeline][13]                                                                      | Asset Pipeline
2011 | nv-lan-6   | [Shiny PC Graphics in Battlefield 3][11] ([on SlideShare][12])                                  |
2010 | stockholm  | [Parallel Futures of a Game Engine v2.0][10]                                                    |
2009 | intel2009  | [Parallel Futures of a Game Engine][9]                                                          |
2009 | siggraph09 | [Parallel Graphics in Frostbite – Current & Future][8]                                          |
2008 | gh2008     | [The Intersection of Game Engines and GPUs: Current & Future][7]                                |
2007 | gdc2007    | [frostbite - Rendering Architecture and Real-time Procedural Shading & Texturing Techniques][6] |

### GDC 2011 & 2012 - Frostbite 2 & Battlefield 3/4

Year | Title                                                                                        | Note
---- | -------------------------------------------------------------------------------------------- | ----
2014 | [Rendering Battlefield 4 with Mantle][17]                                                    | Mantle
2013 | [Battlefield 4 + Frostbite + Mantle][18]                                                     | Mantle
2013 | [Mantle for Developers][19]                                                                  | Mantle
2012 | [Stable SSAO in Battlefield 3 with Selective Temporal Filtering][20]                         | SSAO
2012 | [Terrain in Battlefield 3: A Modern, Complete and Scalable System][21]                       | Terrain
2012 | [Modular Rigging in Battlefield 3][22]                                                       | Animation
2011 | [Approximating Translucency for a Fast, Cheap and Convincing Subsurface Scattering Look][23] | SSS
2011 | [SPU Based Deferred Shading in Battlefield 3 for Playstation 3][24]                          | Deferred-Shading
2011 | [Culling the Battlefield Data Oriented Design in Practice][25]                               |
2011 | [Lighting you up in Battlefield 3][26]                                                       | Lighting
2011 | [DirectX 11 Rendering in Battlefield 3][27]                                                  |


[1]:https://en.wikipedia.org/wiki/Frostbite_(game_engine)
[2]:http://www.dice.se/
[3]:https://www.ea.com/frostbite
[4]:https://www.slideshare.net/
[5]:http://www.techradar.com/news/gaming/from-battlefield-to-fifa-here-s-what-ea-s-frostbite-revolution-means-for-you-1323291
[6]:https://www.ea.com/frostbite/news/frostbite-rendering-architecture-and-real-time-procedural-shading-texturing-techniques
[7]:https://www.ea.com/frostbite/news/the-intersection-of-game-engines-and-gpus-current-future
[8]:https://www.ea.com/frostbite/news/parallel-graphics-in-frostbite-current-future
[9]:https://www.slideshare.net/repii/parallel-futures-of-a-game-engine-2478448
[10]:https://www.ea.com/frostbite/news/parallel-futures-of-a-game-engine-v2-0
[11]:https://www.ea.com/frostbite/news/shiny-pc-graphics-in-battlefield-3
[12]:https://www.slideshare.net/DICEStudio/shiny-pc-graphics-in-battlefield-3
[13]:https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwie7cKFxObsAhXOfXAKHYj3C08QFjAAegQIAxAC&url=https%3A%2F%2Fmedia.contentapi.ea.com%2Fcontent%2Fdam%2Feacom%2Ffrostbite%2Ffiles%2Fscaling-the-pipeline.pptx&usg=AOvVaw2hzoiuZdLIKH3XWD1TyDSe
[14]:https://www.ea.com/frostbite/news/the-rendering-pipeline-challenges-next-steps
[15]:https://www.gdcvault.com/play/1023109/Optimizing-the-Graphics-Pipeline-With
[16]:https://www.gdcvault.com/play/1024612/FrameGraph-Extensible-Rendering-Architecture-in
[17]:https://www.ea.com/frostbite/news/rendering-battlefield-4-with-mantle
[18]:https://www.ea.com/frostbite/news/battlefield-4-frostbite-mantle
[19]:https://www.ea.com/frostbite/news/mantle-for-developers
[20]:https://www.ea.com/frostbite/news/stable-ssao-in-battlefield-3-with-selective-temporal-filtering
[21]:https://www.ea.com/frostbite/news/terrain-in-battlefield-3-a-modern-complete-and-scalable-system
[22]:https://www.gdcvault.com/play/1015573/Modular-Rigging-in-Battlefield
[23]:https://www.ea.com/frostbite/news/approximating-translucency-for-a-fast-cheap-and-convincing-subsurface-scattering-look
[24]:https://www.ea.com/frostbite/news/spu-based-deferred-shading-in-battlefield-3-for-playstation-3
[25]:https://www.ea.com/frostbite/news/culling-the-battlefield-data-oriented-design-in-practice
[26]:https://www.ea.com/frostbite/news/lighting-you-up-in-battlefield-3
[27]:https://www.ea.com/frostbite/news/directx-11-rendering-in-battlefield-3
