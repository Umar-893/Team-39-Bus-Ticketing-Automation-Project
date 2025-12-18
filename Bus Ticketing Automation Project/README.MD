Faisal Movers Transport Booking Assistant – Node Documentation
________________________________________
### 1. Manual Trigger — When clicking ‘Execute workflow’
Triggers the workflow manually for testing.
Purpose: Starts the RAG-loading process when executed.
________________________________________
### 2. Google Drive — Download file
Downloads the CSV file containing Faisal Movers transport data.
Purpose: Supplies raw dataset for embeddings and vector store.
Credentials Required: Google Drive OAuth2
________________________________________
### 3. Default Data Loader
Converts downloaded binary file into a readable document format.
Purpose: Prepares content for Supabase vector insertion.
________________________________________
### 4. Supabase Vector Store (Insert Mode)
Inserts processed documents into the Supabase vector store.
Purpose: Stores embeddings for RAG search.
Credentials Required: Supabase API
________________________________________
### 5. Chat Trigger — When chat message received
Public webhook that triggers the conversational agent for user messages.
Purpose: Starts real-time chat workflow.
________________________________________
### 6. Supabase Vector Store (Retrieve Tool Mode)
Provides RAG search capability to the AI Agent.
Purpose: Returns relevant Faisal Movers data from vector embeddings.
Credentials Required: Supabase API
________________________________________
### 7. Postgres Chat Memory
Stores recent chat interactions for memory-based responses.
Purpose: Allows context persistence across messages.
Credentials Required: Postgres
________________________________________
### 8. Google Gemini/Open Ai Chat Model
Large language model powering the AI Agent.
Purpose: Generates conversational responses based on RAG + rules.
Credentials Required: Google Gemini/PaLM API/Open Ai
________________________________________
### 9. Embeddings — Google Gemini/Open Ai
Generates embeddings for text to be stored in Supabase.
Purpose: Enables semantic search in vector store.
Credentials Required: Google Gemini/PaLM API/Open Ai
________________________________________
### 10. AI Agent
Main decision-making agent following rules:
•	RAG answers
•	Booking mode
•	JSON output for ticket booking
Purpose: Handles all user queries.
________________________________________
### 11. JSON-OUTPUT
Extracts JSON from AI agent responses.
Purpose: Ensures structured booking data is passed to logic nodes.
________________________________________
### 12. Switch
Routes flow based on intent:
•	check_info → general info
•	book_ticket → ticket booking
Purpose: Splits conversation logic.
________________________________________
### 13. Supabase — Fetch Fare Prices
Queries fare based on:
•	source
•	destination
•	bus class
Purpose: Retrieves fare per route.
Credentials Required: Supabase API
________________________________________
### 14. Merge
Combines:
•	Fare details
•	User booking details
Purpose: Prepares unified data for fare calculation.
________________________________________
### 15. Fare_Calculation
Computes:
totalFare = fare_price_pkr * seat_quantity
Purpose: Final fare calculation.
________________________________________
### 16. Stripe — Create Checkout Session
Creates a Stripe checkout page for ticket payment.
Purpose: Payment processing for ticket booking.
Credentials Required: Stripe Secret Key
________________________________________
### 17. Bitly — URL Shortener
Shortens payment URL for user-friendly messaging.
Purpose: Provides clean payment link.
Credentials Required: Bitly API
________________________________________
### 18. Code in JavaScript (Extract Payment Link)
Extracts only the final short link from Bitly response.
Purpose: Sends simplified payment instructions.
________________________________________
### 19. Webhook — Stripe Payment Webhook
Receives Stripe payment confirmation.
Purpose: Validates ticket payment.
________________________________________
### 20. Code1 — Parse Stripe Signature Header
Extracts necessary fields from stripe-signature.
Purpose: Used for signature verification.
________________________________________
### 21. Crypto1 — Verify Stripe Signature
Generates HMAC SHA256 signature.
Purpose: Confirms webhook authenticity.
________________________________________
### 22. If — Signature Validity Check
Checks:
•	signature validity
•	webhook type
•	timestamp freshness
Purpose: Prevents fraudulent callbacks.
________________________________________
### 23. Edit Fields
Extracts customer & ticket details from Stripe webhook.
Fields:
•	Email
•	Name
•	Fare
•	Booking ID
Purpose: Prepare email data.
________________________________________
### 24. Stop and Error
Stops workflow if verification fails.
Purpose: Security enforcement.
________________________________________
### 25. Gmail — Send a message
Sends confirmation email with e-ticket.
Purpose: Delivers ticket to customer.
Credentials Required: Gmail OAuth2

________________________________________
🔐 Credentials Required Overview
Service	Used In Nodes	Credential Required
Google Drive	Download File	Google Drive OAuth2
Supabase	Vector Store (insert + retrieve), Fare Query	Supabase API
Postgres	Chat Memory	Postgres Credentials
Google Gemini / PaLM	Chat Model + Embeddings	Google PaLM API
Stripe	Create Checkout Session, Webhook Validation	Stripe Secret Key
Gmail	Send Email	Gmail OAuth2
Bitly	URL Shortening	Bitly API

