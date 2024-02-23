
# CodeBaristas - Alephium Blockflow Visualizer and Stats Dashboard Overview

## Introduction and Achievements

We are proud to present the Alephium Blockflow Visualizer and Stats Dashboard, a comprehensive suite of tools designed to enhance the understanding and accessibility of the Alephium blockchain. Our project introduces a real-time 3D visualizer application that demonstrates Alephium's sharding algorithm, Blockflow, along with a WebSocket stream that broadcasts newly minted blocks to developers as they occur. Additionally, we have developed a Proof of Concept application that displays static statistics of the Alephium blockchain, and a Python library that facilitates interaction with the Alephium Full Node API and Alephium Explorer API. All components of our project are open-sourced to encourage community engagement and development.

### Access Points:
- **Live Application Links:** 
	- [Blockflow Visualizer](https://visualizer.alph.land)
	- [Stats Dashboard](https://stats.alph.land)

- **Source Code Repository Links:**
	- [Blockflow Visualizer Frontend](https://github.com/CodeBaristas/alephium-visualizer-frontend)
	- [Stats Dashboard Frontend](https://github.com/CodeBaristas/alephium-stats-dashboard-frontend)
	- [Backend](https://github.com/CodeBaristas/alephium-stats-backend)
	- [Backend Client](https://github.com/CodeBaristas/alephium-stats-client)

- **WebSocket Endpoint for Block Streaming:** 
	- wss://alephiumstatsapi.alph.land/streams/blocks

## Technical Architecture

### Frontend 
- **Technologies Used:** NextJS, TypeScript, Mantine, NextUI, React-Three-Fiber, Websockets
- **Deployment Platform:** Vercel
- **Visualizer Core App Functionality:** Our core frontend application allows users to subscribe to our hosted block websocket stream, enabling the visualization of newly minted blocks in a dynamic, 3D space. Each block is color-coded and placed within a circular segment according to its shard, with lines illustrating inter- & intra-group dependencies per the Blockflow algorithm. A unique cylindrical formation emerges from stacking blocks based on their timestamp, offering an engaging and informative user experience. Detailed block information is displayed upon hover, and clicking a block directs users to the Alephium explorer with detailed block data. The interface also includes a logbox for tracking new blocks by hash and offers customizable UI elements and post processing effects for personalization. 

- **Stats Dashboard Functionality:** Our Proof of Concept Stats Dashboard effectively displays static data from the Alephium Explorer API and Full Node API, offering a foundation that can be readily extended for additional functionalities.

- **Outlook:** 3D Performance could be enhanced (Object Pooling, Delete old blocks over a certain threshhold)

### Server 
- **Components:** Dockerized Alephium Stack, Dockerized Backend Application
- **Configuration:** Traefik for flexible routing and service configuration
- **Description:** Our server infrastructure operates on a dockerized Alephium stack, including both the Node and Explorer Backend, alongside our own dockerized application. Through the use of Traefik, we have achieved a highly flexible system configuration, ensuring seamless operation and accessibility of our services.

### Backend 

The backend is developed on the FastAPI framework, specifically chosen for its robust features and efficient development workflow, thus serving as the cornerstone of the hackathon project. Additionally, a shell script is available to generate an OpenAPI client, facilitating seamless integration with the frontend. This generated client provides a structured interface for the frontend to interact with the backend APIs, streamlining the development process and ensuring consistency in communication between the two components.

- **Technologies Used:** FastAPI, SQLModel, OpenAPI, Celery, Redis, PubSub-Websocket, lets-cli
- **Modules:** The backend system comprises two key modules: the WebSocket module and the Stats module. The WebSocket module orchestrates real-time updates to the frontend visualizer (or other clients), employing Celery tasks to regularly monitor for new blocks and leveraging Redis to ensure that each block is transmitted only once. Meanwhile, the Stats module is dedicated to powering the stats dashboard, offering decisive data to the frontend. This module implements security API keys to restrict access to authorized users, with the current key initially set as open (123456) until traffic surpasses a predefined threshold. Additionally, the system implements rate limiting. Interaction with the Alephium Node and Explorer is facilitated through a proprietary Python wrapper, streamlining data retrieval processes and enhancing testability through mock function calls. 

- **Outlook:** While testing efforts have been limited due to time constraints, future iterations are expected to expand testing coverage to enhance system robustness and reliability. Furthermore, plans for future development include the implementation of a database to store bundled stats data, with infrastructure already in place to support this feature when ready for deployment. To address the issue of non-chronological data transmission from the explorer, it's imperative to implement a buffer for each of the 16 chains. These buffers will serve to temporarily store the incoming blocks and subsequently sort them based on their height. Maybe, the explorer itself can be enhanced to ensure that data is sent chronologically.
