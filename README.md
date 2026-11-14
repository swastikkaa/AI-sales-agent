# AI-sales-agent
An AI-powered medical sales outreach agent that discovers doctors and clinics using Google Places API and public web scraping, qualifies leads, generates personalized emails, and manages human-approved outreach campaigns.


AI Sales Agent — Medical Lead Intelligence & Outreach
An AI-assisted sales outreach platform that discovers medical leads, evaluates their fit, creates personalized email campaigns, and manages outreach through a human approval workflow.

Short description
An AI-powered medical sales outreach agent that uses the Google Places API to discover doctors and clinics, then searches publicly available web pages to collect relevant business contact information. It cleans and scores leads against user-defined criteria, generates personalized outreach emails, and supports human-reviewed, trackable email campaigns.
Overview
Finding relevant healthcare professionals and contacting them individually can be time-consuming. This project streamlines that process by bringing lead discovery, lead qualification, personalized messaging, and campaign management into one Streamlit application.

The intended lead-acquisition workflow combines:
1. Google Places API to discover relevant doctors, clinics, and healthcare businesses by location and search criteria.
2. Public-web research and scraping to find publicly listed business contact details and other useful practice information.
3. Lead processing to clean, validate, and organize the collected records before they enter an outreach campaign.
The application also supports CSV uploads, so leads can be imported from an external collection workflow.
Features
- Lead discovery and data collection: Google Places-based business discovery, followed by research of publicly available web pages for contact information.
- CSV lead import: Upload a lead dataset to the dashboard.
- Data cleaning and validation: Standardizes and checks lead data, identifies incomplete records, validates email formats, and removes duplicates.
- Criteria-based lead matching: Evaluate leads using criteria such as city, specialty, clinic type, years of experience, clinic size, patient volume, and email validity.
- Lead quality insights: View profile completeness and criteria match results, including which criteria were met or missed.
- Personalized email generation: Create outreach messages using doctor and clinic details alongside a configurable company, product, value proposition, tone, and call to action.
- LLM and template generation: Use Groq-powered generation when configured, with a deterministic template fallback.
- Human-in-the-loop approval: Review, edit, approve, reject, or reopen drafts before sending.
- Safe email testing: Test mode is enabled by default and routes messages to a configured test inbox.
- Campaign tracking: Store outreach statuses and review campaign activity and metrics.
Workflow
Google Places API
       |
       v
Discover doctors, clinics, and healthcare businesses
       |
       v
Public-web research / scraping
       |
       v
Collect publicly listed business contact information
       |
       v
Clean, validate, deduplicate, and import leads (CSV)
       |
       v
Apply user-defined target criteria
       |
       v
Review qualified leads and profile quality
       |
       v
Generate personalized outreach drafts
       |
       v
Human review: edit / approve / reject
       |
       v
Send approved emails (test mode by default)
       |
       v
Track campaign statuses and results
Tech stack
Area	Technology
Dashboard	Streamlit
Data processing	Python, Pandas, NumPy
Lead discovery	Google Places API
Public contact research	Web scraping / HTTP requests
AI email generation	Groq API (optional)
Email delivery	Resend API or SMTP
Campaign persistence	SQLite
Visualizations	Plotly
Configuration	python-dotenv


Project structure
ai-sales-agent/
├── app.py                      # Main Streamlit application
├── requirements.txt            # Python dependencies
├── .env.example                # Example environment configuration
├── data/
│   ├── doctors_dataset.csv     # Default sample dataset
│   ├── outreach_output.csv     # Outreach export/output
│   └── campaign.db             # Local campaign database (created/used at runtime)
├── src/
│   ├── data_cleaner.py         # Cleaning and validation
│   ├── data_generator.py       # Sample dataset generation
│   ├── data_loader.py          # CSV loading
│   ├── lead_filter.py          # Lead filtering
│   ├── lead_scoring.py         # Criteria matching and data quality
│   ├── email_validator.py      # Email format validation
│   ├── personalization.py      # Campaign and doctor-specific context
│   ├── email_generator.py      # LLM and template email generation
│   ├── message_builder.py      # Outreach message construction
│   ├── email_sender.py         # Resend / SMTP sending and safety checks
│   ├── campaign_store.py       # SQLite campaign state and status transitions
│   ├── outreach_service.py     # Draft, approval, and sending operations
│   └── outreach_ui.py          # Outreach and campaign tracking interface
└── tests/                      # Automated tests
Note: The current application snapshot includes CSV ingestion and the downstream lead/outreach workflow. The Google Places discovery and public-web research steps are part of the lead-acquisition process; configure and run that collection workflow separately if it is not included in your local checkout.
Getting started
1. Clone the repository
git clone <YOUR_REPOSITORY_URL>
cd ai-sales-agent
2. Create and activate a virtual environment
Windows
python -m venv .venv
.venv\Scripts\activate
macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
3. Install dependencies
pip install -r requirements.txt
4. Configure environment variables
Copy the example file:

Windows
copy .env.example .env
macOS / Linux
cp .env.example .env
Edit .env and add the credentials for the services you use. The current example configuration includes:
Variable	Purpose
TEST_MODE	Keep true to route all messages to the test inbox
TEST_RECIPIENT	Your own email address for test delivery
AUTHORIZED_RECIPIENTS	Explicitly permitted addresses for real delivery
RESEND_API_KEY	Resend API key, if using Resend
SENDER_EMAIL	Verified sender address where required
SMTP_HOST	SMTP server host, if using SMTP
SMTP_PORT	SMTP server port
SMTP_USE_TLS	Enable SMTP TLS
SMTP_USER	SMTP username
SMTP_PASSWORD	SMTP password or app password
GROQ_API_KEY	Optional key for AI-generated email content
CAMPAIGN_DB	SQLite campaign database path


Google Places credentials should be kept securely in environment configuration for the lead-discovery workflow. Never commit API keys, email credentials, or your real .env file.
5. Run the application
streamlit run app.py
Open the local URL printed by Streamlit (usually http://localhost:8501).
Using the dashboard
1. Start the Streamlit application.
2. Upload a CSV containing your collected leads, or use the included default dataset.
3. Set target criteria such as city, specialty, clinic type, experience, clinic size, and patient volume.
4. Review the filtered leads, match results, and profile completeness.
5. Configure your company, product/service, value proposition, outreach purpose, tone, and call to action.
6. Generate email drafts and review or edit each message.
7. Approve only the messages you are ready to send.
8. Send approved messages and monitor campaign status in the tracking section.
Lead data
The lead collection process uses Google Places API discovery and publicly available web information to identify relevant practices and business contact details. Only collect information that is publicly available and relevant to legitimate business outreach. Respect website terms, robots directives, applicable privacy and data-protection laws, and API usage policies. Do not bypass access controls or scrape private/personal information.

The repository snapshot also contains a default sample doctors CSV. Treat sample or synthetic records as demonstration data, not verified real-world leads. For real campaigns, import a properly reviewed dataset and validate contact details before outreach.
Email safety
- Test mode is on by default. Configure TEST_RECIPIENT to receive test messages.
- In test mode, messages are routed to the test inbox rather than the doctor's address.
- Real delivery requires deliberately setting TEST_MODE=false and listing permitted addresses in AUTHORIZED_RECIPIENTS.
- Drafts require explicit human approval before sending.
- The campaign store controls message status transitions and prevents a message from being sent again once it has been claimed for sending.
- A successful send response indicates acceptance by the mail provider; it does not guarantee that the recipient read the email or that it reached the inbox.
Use the system responsibly and comply with applicable anti-spam, privacy, and email regulations.
Tests
Run the test suite from the project root:
python -m pytest
If pytest is not installed in your environment:
pip install pytest
python -m pytest
Current scope and future improvements
Potential next steps include:
- Integrate the Google Places and public-web collection workflow directly into the dashboard.
- Improve source attribution and track where each contact detail was found.
- Add stronger email verification beyond format checks.
- Add configurable search locations and discovery limits.
- Expand campaign analytics and delivery reporting.
- Add deployment configuration and operational monitoring.
Security
- Keep .env out of version control.
- Use .env.example only for placeholder values.
- Restrict API keys and email credentials to the minimum permissions required.
- Review lead sources and recipient eligibility before launching a campaign.
- Keep test mode enabled while developing and validating the workflow.
License
Add a license before distributing or reusing this project publicly.
