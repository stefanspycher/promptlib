MCP Anki Server Prompt Specification
You are an expert-level software architect and Python developer specializing in protocol implementation, specifically the Anthropic Model Context Protocol (MCP).

Your task is to write a complete Python FastAPI server that acts as a fully compliant MCP endpoint for the local Anki flashcard application.

Protocol Compliance Requirements:
Manifest: The server MUST serve a valid mcp-manifest.json file at its root (/).

Function Execution: The server MUST expose tool functions at the POST /functions/<function_name> path as per the MCP specification.

Server Specification:
1. MCP Manifest (mcp-manifest.json)
The manifest file you generate should define the following:

protocol_version: "1.0"

display_name: "AnkiConnect MCP"

description: "A server that allows an AI to interact with the Anki flashcard application to analyze cards, review statistics, and conduct study sessions."

auth: {"type": "none"}

functions: The list of all four function definitions below.

2. MCP Function Definitions:
You will implement the following four functions:

Function 1: analyze_card
name: "analyze_card"

description: "Analyzes a single Anki card for didactic and linguistic quality using the 'strict professor' persona for its analysis."

input_schema: A JSON Schema object that requires a single property: card_id (type integer).

Implementation Logic: Use anki-connect to fetch the card data, then pass it to an external LLM (Gemini) for analysis, and return the analysis.

Function 2: get_deck_overview
name: "get_deck_overview"

description: "Retrieves key statistics for an Anki deck and provides a qualitative diagnosis of its health."

input_schema: A JSON Schema object that requires a single property: deck_name (type string).

Implementation Logic: Use anki-connect to fetch deck statistics, pass those stats to an external LLM (Gemini) for diagnosis, and return the diagnosis.

Function 3: analyze_deck
name: "analyze_deck"

description: "Analyzes all cards in a specified Anki deck for quality. Returns a summary of findings. This can be a long-running operation."

input_schema: A JSON Schema object that requires a single property: deck_name (type string).

Implementation Logic: Use anki-connect findCards action with the deck name. Loop through the returned card IDs, passing each to the LLM for analysis. Aggregate the results and return a summary.

Function 4: review_lapsed_cards
name: "review_lapsed_cards"

description: "Fetches cards that the user failed (lapsed) within a recent number of days, so they can be reviewed."

input_schema: A JSON Schema object with two required properties: deck_name (type string, can be '*' for all decks) and days (type integer).

Implementation Logic: Use anki-connect findCards action with a query like deck:<deck_name> rated:1:<days>. Fetch the content (front and back) for each resulting card ID and return it as a list of objects.

Your final output will be the complete, single-file Python code for this FastAPI server.