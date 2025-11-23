# L14a: A Crash Course in Networking
In this lecture, we will cover the basics of computer networking, including key concepts, protocols, and technologies that enable communication between devices over a network.

> __Learning Objectives:__
> 
> By the end of this lecture, you should be able to:
> 
> * __Network Protocols and Communication__: Understand the network protocols (TCP, UDP, HTTP/HTTPS, FTP) that enable data exchange between computers. Learn how these protocols operate at different layers of the Internet and how they balance reliability, speed, and overhead.
> * __Web Services and APIs__: Comprehend what web services are and how Application Programming Interfaces (APIs) define the rules for accessing them. Understand how APIs act as intermediaries between clients and backend services, handling authentication, validation, and data serialization.
> * __RESTful Web Services__: Learn the principles of REST (Representational State Transfer) as an architecture for building web services. Understand the request-response cycle, HTTP methods (GET, POST, PUT, DELETE), status codes, and how to construct and consume RESTful APIs.

Let's get started!

___

## Web Services and APIs
A web service is software that lets machines interact over a network. Application programming interfaces (APIs) are the rules used to access these services.
* An API specification enables communication between different computer systems and allows clients to request services or data over the Internet. Think of the API as the rules and capabilities of the communication system.
* A web service has a public-facing API and internal APIs to manage its features and data. Users call the public API methods, which in turn call the private API methods. Data is returned from the public API methods.
* A web service uses a communication protocol like the Hypertext Transfer Protocol (HTTP) to exchange data over the Internet. It can transfer data in various formats, such as JSON, CSV, and XML.

Let's explore the network protocols that enable web services to function effectively.

### Common Network Protocols

These protocols form the foundation of the Internet and web service communication by defining how data is exchanged between clients and servers.

Let's watch a short video that summarizes some of the most common network protocols used in web services: [click here to watch the video](https://www.youtube.com/watch?v=P6SZLcGE4us).

* **SMTP (Simple Mail Transfer Protocol):** A text-based protocol for sending and relaying email between mail servers, specifying how outgoing messages are handed off through the mail-delivery system.
* **TCP (Transmission Control Protocol):** A connection-oriented transport protocol that guarantees reliable, in-order delivery of a byte stream via handshake, acknowledgments, and retransmissions.
* **UDP (User Datagram Protocol):** A connectionless transport protocol sending discrete packets (datagrams) with minimal overhead, offering low latency but without guarantees of delivery, ordering, or duplicate protection.
* **FTP (File Transfer Protocol):** A client-server protocol over TCP for browsing, uploading, and downloading files; supports both anonymous and authenticated sessions.
* **HTTP/HTTPS (HyperText Transfer Protocol / HTTP Secure):** The application-layer protocol for fetching web resources, with HTTPS adding TLS encryption for confidentiality and integrity.
  * **GET:** Retrieves a resource without modifying it.
  * **POST:** Submits data to create or update a resource (e.g., form data).
  * **PUT:** Replaces an existing resource or creates it if it does not exist.
  * **DELETE:** Removes a specified resource.
* **WebSockets:** A full-duplex, bidirectional protocol over a single TCP connection, enabling low-latency, real-time message exchange (e.g., chat or live feeds).


Now that we have a basic understanding of standard network protocols, let's jump into the concept of RESTful web services, a key component of modern web applications. 

___

## RESTful Web Services
RESTful web services are a type of web service that follows the principles of REST (Representational State Transfer). REST is an architecture for creating stateless web services.

The REST model functions through a request-response cycle, where a client (such as a web browser or mobile app) sends a request to a server, and the server responds with the requested data or an appropriate status code. Communication typically occurs over HTTPS.

* __Interoperability__: RESTful APIs are based on standard protocols like HTTP, making them compatible with many applications and devices.
* __Flexibility__: RESTful APIs can communicate using any data format. JSON has become the standard data interchange format (though older formats such as XML are still in use).
* __Scalability__: RESTful APIs can handle large volumes of requests and data transfer without significant performance degradation.


The URLs published by the Application Programming Interface (API) are often called _endpoints_. Each endpoint represents a specific resource or group of resources, and the operations on these resources are determined by the HTTP methods used in the requests.

For an overview of RESTful web services, [watch this short video](https://www.youtube.com/watch?app=desktop&v=-mN3VyJuCjM).

Let's break down the components of a RESTful web service:

1. __Client → REST API__: The client (browser, mobile app, etc.) sends an HTTPS request to an API endpoint. The client selects an HTTP method to perform the desired Create/Read/Update/Delete (CRUD) action on a _resource_ (e.g., `/users/42`):
    - __GET__: retrieve data from the resource, meaning you are requesting information about the resource.
    - __POST__: create a new resource, meaning you are submitting data to the server to create it.
    - __PUT__: update an existing resource, meaning you are sending data to modify it.
    - __DELETE__: delete a specified resource, meaning you are requesting the server to remove it.
  
    Any payload required for the request is typically encoded in JSON or older formats, such as XML. This can include user authentication, such as identity and permissions data.

2. __REST API Internals__: Internally, the API layer handles the client request: 
    - **Routing & Controller**: Maps the endpoint to an appropriate handler function.
    - **Validation & Business Logic**: The handler function checks the authentication and authorization of the client request, meaning is this client known and allowed to access the resource? This step also validates client inputs, for example, by checking if the data provided in a POST request is in the correct format and meets the required criteria. 
    - **Serialization**: Converts incoming JSON data into domain objects, and later converts objects back into JSON.

    The internal API logic is implemented by the server developers, who write the code that handles requests and prepares responses. The client doesn’t need to know the details of this implementation; they need to know what to send and what to expect in return.

3. __REST API → Backend services__: The functions that handle the request may need to access data or perform actions that require communication with lower-level backend services. They may call other APIs, databases, or microservices to retrieve or manipulate data. This is where the API layer acts as an intermediary between the client and the backend services.
    - __Backend__: The API layer invokes lower-level backend services (such as a database, a microservice, or a message queue). These calls are typically language- or protocol-specific (such as SQL over TCP) rather than HTTP.
    - __Data__: The backend returns raw data or status codes to the API layer. This data is not yet in a format that the client will understand or expects.

4. __Server → REST API → Client__: Now we need to send the data back to the client in the format promised by the API specification.
    - __Process__: The API server receives the backend's result, converts it into a JSON response, and attaches an HTTP status code (200, 201, 404, 500, etc.). Code `200` indicates success, `201` indicates a resource was created, `404` indicates the resource was not found, and `500` indicates an internal server error. The API layer serializes the response data into JSON format, a lightweight data interchange format that is easy for both humans and machines to read and write.
    - __Response__: The API sends the response back to the client over HTTPS with the status code and any requested or created data.

Let's look at a few examples of the RESTful API request and response cycle.

> __Examples__
> 
> [▶ Let's download forecast data from a RESTful Web Service for the National Weather Service (NWS)](CHEME-5800-L14a-Example-RESTful-NWS-Fall-2025.ipynb). In this example, we will build a simple SDK in Julia to interact with the National Weather Service's RESTful API. We will cover how to make HTTP requests, handle responses, and parse JSON data to retrieve weather information.
>
> [▶ Let's download a genome scale metabolic network model from UCSB](CHEME-5800-L14a-Example-RESTful-UCSB-MetabolicNetworks-Fall-2025.ipynb). In this example, we will demonstrate how to use RESTful web services to access and download metabolic network data from the UCSB Bioinformatics Resource. We will explore how to construct API requests, handle authentication, and process the retrieved data for further analysis.

___

## Summary
In this lecture, we covered the basics of computer networking and web services, focusing on how APIs enable communication between systems.

> __Key Takeaways:__
>
> * **Network protocols enable reliable communication**: TCP provides guaranteed delivery, while UDP offers lower latency with minimal overhead. HTTP/HTTPS adds application-layer functionality for web services, with HTTPS providing encryption.
> * **Web services and APIs abstract complexity**: APIs act as intermediaries between clients and backend services, handling authentication, validation, and serialization. This allows developers to interact with complex systems through simple endpoints.
> * **RESTful web services provide a standard architecture**: REST uses HTTP standards and familiar web protocols. The request-response cycle with standard HTTP methods and status codes makes RESTful APIs compatible across different programming languages and platforms.

___
These networking concepts and web service patterns are the foundation for building distributed systems where different applications communicate over the Internet.
