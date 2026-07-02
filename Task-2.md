# Task 3 - Integration Design

## Integration Architecture

When a user submits the consultation form on the landing page, the frontend first validates the form fields (Name, Phone Number, and Clinic Preference). Once validation is successful, the frontend sends the form data to a backend API using an HTTPS POST request.

The backend receives the request and becomes the central point for all integrations. First, it checks whether the submitted phone number already exists in HubSpot. Since this landing page does not collect email addresses and HubSpot primarily deduplicates contacts using email, I would implement phone-number-based deduplication in the backend. If the phone number already exists, the existing contact is updated. Otherwise, a new contact is created.

The contact is stored in HubSpot with the following properties:

- Name
- Phone Number
- Clinic Preference
- Source = "Google Ads - Consultation Landing Page"
- Lead Status = "New Enquiry"

After HubSpot successfully processes the contact, the backend sends a request to the Karix WhatsApp Business API to deliver a confirmation message to the patient. Once the message request is accepted, the backend records the response for monitoring purposes.

Finally, after the lead has been successfully processed, the frontend fires the **appointment_booking_completed** event through Google Tag Manager. GTM forwards the event to Google Analytics 4, and the same conversion is imported into Google Ads for campaign optimization.

---

# Biggest Failure Point and Fallback Strategy

The biggest failure point in this integration is contact creation or update in HubSpot. If HubSpot is temporarily unavailable or its API fails, valuable patient leads could be lost.

To avoid this, every form submission should first be stored in the backend database before attempting any external API calls. If HubSpot returns an error, the lead remains safely stored and a background retry mechanism can automatically resend the data once the service becomes available. This ensures that no patient enquiry is permanently lost.

---

# WhatsApp SLA (Within 2 Minutes)

The WhatsApp confirmation depends on the availability of the backend server, the Karix API, and the internet connection. Delays can occur because of API failures, server downtime, network issues, or high request volume.

To maintain the 2-minute SLA, the backend should log every WhatsApp request along with its status and response time. Failed requests should be automatically retried, and monitoring tools should generate alerts whenever message delivery exceeds the expected time limit. Regular logging and monitoring ensure that any delivery issues are detected quickly and resolved before they impact patient communication.