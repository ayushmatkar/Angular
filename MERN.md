Project Overview & Integration Guide
A simplified explanation of the entire Frontend + Backend architecture and how they work together.

1. Frontend (The User Interface)
Built with: React (TypeScript), Redux Toolkit, Material UI (MUI).

Key Concepts
Hooks: We use React hooks to manage logic inside components.
useState: To handle local data (e.g., form inputs in 
CreateTicketModal
).
useEffect: To run code when a page loads (e.g., fetching tickets in 
RequesterDashboard
).
useAppDispatch / useAppSelector: Custom Redux hooks to send actions and read global state.
Redux (State Management): Used to store data globally so it's accessible everywhere.
authSlice: Stores the logged-in user and token.
ticketSlice: Stores the list of tickets for Requesters.
agentSlice: Stores tickets assigned to Agents.
Material UI: A component library for beautiful, pre-built designs.
We use Card, Button, TextField, Dialog (Modals), and Chip (Status tags) to build the UI quickly.
Component Structure
Pages: Main screens like 
Login
, 
RequesterDashboard
, 
TicketDetails
.
Components: Reusable parts.
CreateTicketModal
: The popup form for new tickets.
Navbar: Top navigation bar.
StatusChip: A small label showing if a ticket is "Open" or "Closed".
2. Backend (The API)
Built with: Node.js, Express, MongoDB (Mongoose).

Key Concepts
Endpoints: URLs that the Frontend calls to get or save data.
POST /api/auth/login: Checks password and returns a token.
POST /api/tickets: Creates a new ticket.
GET /api/agent/tickets/assigned-to-me: Gets tickets for the logged-in agent.
Middleware: Functions that run before the main logic.
authMiddleware
: Checks if the user is logged in (verifies JWT token).
roleMiddleware: Checks if the user is an Agent or Requester.
upload
: Handles file uploads (images/PDFs) before saving the ticket attachment.
3. Integration Workflow (End-to-End)
How the Frontend and Backend talk to each other.

Example: Creating a Ticket
Frontend Action:

User fills out the "Create Ticket" form.
React creates a JSON object: { title: "Help", priority: "high", ... }.
ticketService.createTicket() sends this JSON to the backend.
Request (The Journey):

The request initializes POST /api/tickets.
It carries a Header: Authorization: Bearer <token> (proof of identity).
Backend Processing:

Middleware: "Is this token valid?" -> "Yes".
Controller: Receives the data.
Service: Saves the ticket to the MongoDB database.
Response: Sends back 201 Created with the new Ticket ID.
Frontend Update:

React receives the success message.
It closes the modal and refreshes the Dashboard list to show the new ticket.
Example: File Upload
For attachments, it's a 2-step process:

Step 1: Create the Ticket (as above) -> Get ticketId.
Step 2: The Frontend automatically calls POST /api/attachments/tickets/:id with the file.
The Backend saves the file to the uploads/ folder and links it to the database.
4. Why This Architecture?
Separation: Frontend deals with "looks", Backend deals with "data". You can update one without breaking the other.
Security: The Backend never trusts the Frontend. It verifies every single request using the Token.
Scalability: The "Service Layer" in the backend keeps business logic organized, so the app can grow huge without becoming messy.
