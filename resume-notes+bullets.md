# Work

## Racecar

### Context/Details

Data steaming platform designed for high-throughput data and real time retrieval via Kafka, S3, and NATS

### Resume Bullets

- Modernized the web build pipeline by configuring Vite and Nginx to handle pre-compressed Gzip and Brotli assets, offloading server-side compression overhead. 
- Implemented application-wide lazy loading and code splitting based on bundle analysis, significantly reducing initial payload size.
- Diagnosed and resolved rendering bottlenecks using Chrome DevTools, achieving a 4x reduction in FCP/LCP metrics. Eliminated Cumulative Layout Shift (CLS) by implementing asset preloading strategies and refactoring inefficient Vue.js watchers to minimize main-thread blocking.
- Spearheaded a complete visual modernization of the legacy interface, conducting a feature audit to consolidate redundant utilities and declutter the UI. Established a cohesive design system that reduced user cognitive load and streamlined critical navigation paths for non-technical stakeholders.
- Architected a robust GitLab CI/CD pipeline to enforce the new performance standards, integrating automated Lighthouse regression testing and containerized (Docker) builds. This automation reduced deployment friction and ensured continuous monitoring of Core Web Vitals.

### Newer developments

I owned development of a search feature for our data platform (which started life as its own app that I integrated as a page into our main one while allowing it to be standalone via some plugins) that allowed for integration with an AI chatbot to browse and produce results from the data flowing through the platform. This involved multiple levels of visialization: google images like view for our vector image database, map view for plotting data with hoverable images, and an table to see raw data. This also stored the chat history and llm responses to the user and had a modern feel akin to perplexity.

I am owning development of our 2.0 web release, completely overhauling the UI for a modern look and feel, making it more accessible and responsive by designing for smaller screen sizes and ensuring tab completion works for navigation (and adding developer keybinds that I feel are super helpful). This also involves coordinating with our subcontractor and delegating tasks to them to get this release done.

## Hercules

### Context/Details

Hercules was designed to leverage autobahn's data streaming caipabilities to read off of a continuous stream of data and train object detection models incrimentally as data came in, using new data to refine repeatedly. I mainly worked on data storage/visualization as well as a little inter-process communication in python with redis for our 3 step data processing pipeline 1. acquire data from the stream 2. convert the data to COCO format, and 3. train on the data.

### Resume Bullets

- Engineered an automated incremental learning pipeline capable of processing continuous data streams for real-time object detection model refinement. Implemented a Python-based ingestion pipeline process to ingest, normalize, and convert high-velocity data into COCO format, processing tens of thousands of images daily.
- Architected a scalable Inter-Process Communication (IPC) layer using Redis to orchestrate the asynchronous training workflow (Acquisition, Transformation, Training). This decoupled design ensured high availability and prevented data loss during high-throughput ingestion spikes.
- Developed a real-time command dashboard using Vue.js and Apache ECharts to visualize live training metrics (loss/MAP scores) and model convergence. Leveraged PostgreSQL for persistent metadata storage, providing immediate insight into the performance of incrementally trained models.

## SkySnare - Drone Defense System

### Context/Details

Continued to work on this system from internship.
Cut down system to core components for U of A use, removing camera, omni antenna, and smaller nodes. 
Added some extra UI features to make it more robust (multiple base stations, more efficient graphing, etc)
Assisting in deployment to the U of A
Working on identifying drones via wifi Channel State Information passively

### Resume Bullets

- Leading the software and web infrastructure evolution, managing the Django/React stack to ensure reliable ingestion and visualization of telemetry data from new Software-Defined Radio (SDR) sources.
- Orchestrating the software integration for on-site university deployments, coordinating directly with campus security teams to interface the detection platform with their existing network infrastructure.
- Collaborating on R&D initiatives to detect non-broadcast drones using WiFi Channel State Information (CSI), expanding the system's capabilities beyond standard Remote ID compliance.

# Projects

## Skysnare

### Context/Details:

Skysnare is a drone tracking system that consists of:

- a station with an omnidirectional antenna, directional antenna and camera that get pointed at the drone in flight
- smaller nodes that extend the range of the base larger station
- a web application that allows for monitoring of an area via a map and for the system to be controlled remotely

The system tracks the drones by utilizing Remote ID, a standard by the FAA requiring drones to broadcast their location via bluetooth and wifi as they are in flight.

My development was focused on the backend in Django and the web application in React. Utilizing websockets, I was able to connect the station to Django hosted on AWS, then to our web application. All of the captured drone and station data was also stored in an SQLite database as it went through Django, as well as one camera frame per second. One of the larger features I developed on the web app was the playback mode. This allowed the user to select a time window and go through the window to see historical drone movements detected by our system.

### Resume Bullets

- Co-Architected a distributed FAA Remote ID drone tracking system, utilizing a network of hardware nodes for signal triangulation and a central base station for visual verification.
- Engineered a real-time React frontend deployed on AWS (EC2), implementing WebSocket connections for live flight path mapping and a historical playback engine for flight analysis.
- Built the backend control plane using Django, designing robust APIs for telemetry ingestion and orchestrating data storage for millions of flight data points in SQLite/Postgres.
- Delivered a comprehensive ["Tech Talk"](https://github.com/jacobghunter/SkySnare/blob/main/Final_Presentation.pdf) to company leadership, presenting the architectural challenges and successful implementation of the multi-node mesh network.

## Chess CNN

Context: 

### Context/Details

- This project was mostly about data preprocessing and transformation. Because of the limited time and compute, the most important part was figuring out how to process the data. I ended up going through many levels of iteration as I problem solved, starting with raw PGN games at a high level with a 12x8x8 bitboard I also tried 24x8x8 and 5x12x8x8 (for the top 5 predicted moves). 
- This produced useless moves, so I worked on masking out the data in the preprocessing step, putting -1s where pieces couldnt move on each bitboard. This still left some issues with accuracy, so I tried making a dataset where the top 5 stockfish moves were evaluated at each position and their pawn values that it gave. 
- I then weighted the guesses based on those values, scaling them based on the best move (which was given a value of 1 since it was correct). The model was then made more complex when I looked at some data on AlphaZero and saw they used residual blocks, which I tried and ended up with three of. 
- I also early on settled on only training it on white and only giving it white positions to hopefully simplify things. I also swapped to h5 files somewhere in the middle to speed up training significantly as it allowed dynamic loading of the numpy array data so it didnt have to load everything into VRAM at once and it could grab based on the batch size. 
- I did also implement a simple custom loss function to combine the loss for the model choosing the correct piece to move and the correct place to move it, which just cross entropied both and scaled them. 
- Choosing the correct piece and making the correct move ended up being quite challenging to do simultaniously, so it was very mediocre at both. Towards the end it ended up making alright openings and at least playing valid moves, but fell apart after move 6 or 7 most of the time. It would have a few flashes of brilliance with pretty good moves and would then sack its queen on the next turn.

### Resume Bullets

- Engineered a high-performance data pipeline to process millions of chess positions, utilizing HDF5 for dynamic, out-of-core data loading to bypass VRAM limitations and accelerate training throughput.
- Designed a custom Convolutional Neural Network (CNN) architecture integrating Residual Blocks (inspired by AlphaZero) and a specialized multi-component loss function (Piece Selection + Move Location) to optimize move prediction accuracy.
- Implemented a novel data transformation strategy, generating training labels weighted by Stockfish engine evaluations (centipawn loss) and enforcing rule compliance via a move-masking layer to zero out illegal moves during inference.
- Optimized the training dataset by standardizing perspective (White-to-move only) and utilizing bitboard representations (12x8x8), significantly reducing the dimensionality required for model convergence on resource-constrained hardware.


## Keyboard

Built using ergogen for the layout, kicad for the wiring and PCB design, freecad for the case/plate design, and zmk for the firmware. Using klipper + orca for my 3d printer. PCB printed and hand soldered all components, using a nice! view display on the left, 5000mah batteries in both sides, two rotary encoders, a tps43 touchpad and choc v1 switches to keep it low profile.