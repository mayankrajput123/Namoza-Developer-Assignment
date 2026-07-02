Task 03 – Integration Design
Integration Architecture

I would use a direct backend API integration instead of HubSpot Forms Embed, Zapier, or Make. A direct API approach provides better security, lower latency, greater flexibility, and full control over validation, error handling, and logging.

The user submits the consultation form (Name, Phone Number, and Clinic Preference) on the landing page. The frontend sends this data via an HTTPS POST request to a Node.js/Express backend API. The backend validates and sanitizes the input before processing it.

The backend then searches HubSpot CRM using the CRM Search API with the submitted phone number. This step is important because HubSpot's default deduplication works on email, not phone number. Since the landing page does not collect email addresses, I would explicitly search by the phone property. If a matching contact exists, the backend updates the existing contact with the latest information. If no record is found, it creates a new contact with the following properties:

Name
Phone Number
Clinic Preference
Source = "Google Ads - Consultation Landing Page"
Lead Status = "New Enquiry"

After the contact is successfully created or updated, the backend sends a request to the Karix WhatsApp Business API to deliver an automated enquiry confirmation message. Once the backend confirms successful processing, the frontend triggers the GTM event consultation_form_submitted, which is sent to GA4 and imported into Google Ads as the primary conversion for campaign optimization.

Biggest Failure Point and Fallback

The biggest failure point is a successful form submission but a failed HubSpot API request caused by network issues, API downtime, or rate limits. To handle this, I would implement retry logic with exponential backoff and place failed requests into a persistent queue for automatic retries. Every request would be logged with a unique request ID so that failures can be traced and manually reprocessed if necessary.

WhatsApp SLA Monitoring

The two-minute WhatsApp SLA could be affected by backend failures, HubSpot delays, Karix API issues, or network interruptions. To ensure compliance, I would send the WhatsApp request asynchronously immediately after the CRM update, monitor API response times, maintain delivery logs, and configure alerts for failed or delayed messages. A monitoring dashboard tracking queue length, retry count, API latency, and message delivery status would help identify problems quickly and ensure patients receive timely confirmation messages.