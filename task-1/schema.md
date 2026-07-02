Task 01 – GTM Event Schema
1. GTM Event Schema
Event Name	Trigger Type	Key Parameters (Minimum 3)	GA4 Report / Audience
page_view	Page View Trigger	page_title, page_location, page_path	Pages & Screens Report
clinic_page_view	Page View (Clinic Pages)	clinic_name, city, page_url	Clinic Performance Report
booking_step_complete	Custom Event (dataLayer Push)	step_number, step_name, clinic_location	Funnel Exploration
booking_completed	Custom Event	booking_id, clinic_location, specialty	Conversion Report
call_now_click	Click Trigger (tel:)	phone_number, page_name, clinic_location	Call CTA Audience
whatsapp_click	Click Trigger (wa.me)	page_name, clinic_location, button_location	Engagement Report
patient_guide_form_submit	Form Submission	patient_name, phone_number, guide_name	Lead Generation Report
patient_guide_download	File Download Trigger	file_name, file_type, page_name	File Download Report
consultation_form_submit	Form Submission	name, phone, clinic_location	Conversion Report
blog_scroll_25	Scroll Depth (25%)	article_title, scroll_percent, category	Blog Engagement
blog_scroll_50	Scroll Depth (50%)	article_title, scroll_percent, category	Blog Engagement
blog_scroll_75	Scroll Depth (75%)	article_title, scroll_percent, category	Engaged Readers Audience
blog_scroll_100	Scroll Depth (100%)	article_title, scroll_percent, category	Fully Engaged Readers
form_error	Form Validation Error	field_name, error_message, form_name	Form Error Analysis
2. Booking Funnel Tracking

The appointment booking process has three steps:

Select Clinic & Specialty
Enter Name, Phone & Preferred Date
Confirm Booking

Each completed step pushes a custom event to the dataLayer. GTM listens for these custom events and forwards them to GA4, allowing Funnel Exploration to measure where users drop off.

Step 1 – Clinic & Specialty Selected
GTM Trigger

Custom Event Trigger

booking_step_complete
dataLayer Push
window.dataLayer = window.dataLayer || [];

window.dataLayer.push({
  event: "booking_step_complete",
  step_number: 1,
  step_name: "location_specialty_selected",
  clinic_location: "Bengaluru",
  specialty: "Orthopaedics"
});
Step 2 – Patient Details Entered
GTM Trigger
booking_step_complete
dataLayer Push
window.dataLayer.push({
  event: "booking_step_complete",
  step_number: 2,
  step_name: "patient_details_entered",
  clinic_location: "Bengaluru",
  specialty: "Orthopaedics",
  preferred_date: "2026-07-10"
});
Step 3 – Booking Confirmed
GTM Trigger
booking_completed
dataLayer Push
window.dataLayer.push({
  event: "booking_completed",
  booking_id: "BK102345",
  clinic_location: "Bengaluru",
  specialty: "Orthopaedics",
  booking_status: "Confirmed"
});
3. GTM Trigger Configuration
Booking Step	Trigger Type	Fires When
Step 1	Custom Event	User selects clinic and specialty and clicks "Next"
Step 2	Custom Event	User enters patient details and proceeds
Step 3	Custom Event	Booking confirmation is successful
4. GA4 Funnel Exploration

A closed funnel will be created in GA4 Explore → Funnel Exploration using these events:

Funnel Step	Event
Step 1	booking_step_complete (step_number = 1)
Step 2	booking_step_complete (step_number = 2)
Step 3	booking_completed

This setup allows the marketing team to identify where users abandon the booking flow. For example, if many users complete Step 1 but not Step 2, the patient details form may be too long or difficult to complete. If users reach Step 2 but fail to confirm, there may be issues with date selection, validation, or booking confirmation. GA4's Funnel Exploration report will display the conversion rate and abandonment rate between each step, helping optimize the booking experience.

5. Google Ads Conversion Action
Selected Conversion
consultation_form_submit
Why This Conversion?

I would import consultation_form_submit into Google Ads because it represents the primary business goal: generating a qualified patient enquiry. Unlike clicks on the Call Now or WhatsApp buttons, submitting the consultation form captures complete lead information that can be followed up by the clinic. It is a stronger indicator of purchase intent and directly contributes to appointment bookings. Optimizing Google Ads campaigns around this conversion allows Smart Bidding to target users who are more likely to become actual patients, improving campaign performance and return on ad spend.

6. Why Custom dataLayer Pushes Are Required

Google Tag Manager cannot automatically detect progress through a multi-step booking form. The frontend developer must push custom events into the dataLayer whenever a user completes each step. GTM listens for these events and sends them to GA4. Without these custom dataLayer.push() events, GTM would only know that the page loaded—it would not know which booking step the user reached or where they abandoned the process.