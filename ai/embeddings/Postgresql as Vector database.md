# Postgresql as Vector Database.

## What is Vector Database ?
A vector database is a collection of data that is stored as mathematical representations. A vector database is a type of database that stores and manages unstructured data, such as text, images, or audio, in vector embeddings (high-dimensional vectors) to make it easy to find and retrieve similar objects quickly.
Vector databases are designed to store and query high-dimensional vectors. These vectors are mathematical representations of objects or data points in a multi-dimensional space.
Vector databases make it easier for machine learning models to remember previous inputs. This allows machine learning to be used to power search, recommendations, and text generation use-cases.
Vector databases are different from traditional relational databases and NoSQL databases. Traditional relational databases, such as PostgreSQL, store tabular data in rows and columns. NoSQL databases, such as MongoDB, store data in JSON documents. Vector databases are designed to handle one specific type of data: vector embeddings

A vector database uses a combination of different algorithms that all participate in Approximate Nearest Neighbor (ANN) search. These algorithms optimize the search through hashing, quantization, or graph-based search.

There are two types of vector databases
1.  Dedicated Vector Databases.
2.  Databases that support vector search.

A vector database is not a actual graph database.

### Vector Indexes Vs Vector Databases
Its important to note that vector index and vector databases are different.
Standalone vector indices like FAISS (Facebook AI Similarity Search) can significantly improve the search and retrieval of vector embeddings, but they lack capabilities that exist in any database. Vector databases, on the other hand, are purpose-built to manage vector embeddings, providing several advantages over using standalone vector indices

## Why is Vector Database useful ?
- Efficient handling of high-dimensional data
- Enhanced search capabilities
- Scalability
- Speed and accuracy
- Improved machine learning and AI integration
- Facilitates advanced analytics and insights
- Long term memory of LLMs

## How does Vector Database Search Work ?
Vector databases use special search techniques known as Approximate Nearest Neighbor (ANN) search, which includes methods like hashing and graph-based searches.
These algorithms are assembled into a pipeline that provides fast and accurate retrieval of the neighbors of a queried vector. Since the vector database provides approximate results, the main trade-offs we consider are between accuracy and speed. The more accurate the result, the slower the query will be. However, a good system can provide ultra-fast search with near-perfect accuracy.

![Pipeline](https://github.com/pawankulkarni13/Articles-2024/assets/10267341/b9772320-b00a-4ae4-8bad-2d10a3a38153)

1. Indexing: The vector database indexes vectors using an algorithm such as PQ, LSH, or HNSW (more on these below). This step maps the vectors to a data structure that will enable faster searching.
2. Querying: The vector database compares the indexed query vector to the indexed vectors in the dataset to find the nearest neighbors (applying a similarity metric used by that index)
3. Post Processing: In some cases, the vector database retrieves the final nearest neighbors from the dataset and post-processes them to return the final results. This step can include re-ranking the nearest neighbors using a different similarity measure.

## Different Types of Vector Database.
Dedicated types are - 
1. Chroma
2. Pinecone
3. Weaviate
4. Faiss
5. Qrant

## What are Embeddings ?
Embeddings, in the context of OpenAI, are numerical representations of textual or code-based information. They convert concepts into number sequences, which enables computers to comprehend the relationships between these concepts more easily.
They are represented as a vector part of input/value.Embeddings are used to measure the relatedness of text inputs. They allow for the comparison of similar text inputs by measuring the distance between their respective vectors. 

Depending on the data/input - types of embeddings vary from text to image to graph.

## End to End Diagram
![Diagram](https://github.com/pawankulkarni13/Articles-2024/assets/10267341/9970434e-2caa-4d16-934a-55061dbfdfe1)

## Vector Embeddings
A vector embedding is a numerical representation of data that captures semantic relationships and similarities, making it possible to perform mathematical operations and comparisons on the data for various tasks like text analysis and recommendation systems.
An embedding is a special format of data representation that machine learning models and algorithms can easily use. The embedding is an information dense representation of the semantic meaning of a piece of text. Each embedding is a vector of floating-point numbers, such that the distance between two embeddings in the vector space is correlated with semantic similarity between two inputs in the original format. For example, if two texts are similar, then their vector representations should also be similar.


## Postgresql as Vector Database
Postgresql support vectors via a extension.

### pgvector
pgvector is an open-source vector similarity extension. It contributes to: Search for the exact and approximate nearest neighbors, L2 distance, inner product distance, and cosine distance for each language that has a Postgres client.

[Link](https://github.com/pgvector/pgvector)

### Advantages
Here are some advantages of pgvector: 
It allows vector data to exist alongside other data in Postgres.
It has better query performance than IVFFlat.
It doesn't require a training step like IVFFlat.

