# Task 01 — GTM Event Schema (OrthoNow)

## Overview

This document defines the Google Tag Manager (GTM) event tracking plan for the OrthoNow website. These events will help track user interactions and send meaningful data to Google Analytics 4 (GA4) for reporting, funnel analysis, audience creation, and conversion tracking.

## GTM Event Schema

| Event Name| Trigger Type | Key Parameters | GA4 Purpose |
|----------------------------|--------------|----------------|-------------|
| call_button_clicked | Click | phone_number, page_name, button_location | Measure call engagement |# Task 1 - GTM Event Tracking Schema

## Overview

This document defines the Google Tag Manager (GTM) event tracking plan for the OrthoNow website. The purpose of this schema is to track important user interactions, measure user engagement, analyze the booking funnel, and send meaningful data to Google Analytics 4 (GA4) for reporting, audience creation, and conversion tracking.

---

# GTM Event Schema

| Event Name | Trigger Type | Key Parameters (Minimum 3) | GA4 Purpose |
|------------|--------------|----------------------------|-------------|
| call_button_clicked | Click | phone_number, page_name, button_location | Measure call engagement |
| whatsapp_clicked | Click | page_name, whatsapp_link, button_location | Track WhatsApp interactions |
| patient_guide_downloaded | Form Submit + File Download | guide_name, user_name, phone_number | Measure lead generation |
| clinic_page_viewed | Page View | clinic_name, city, page_url | Analyze clinic page performance |
| blog_scroll_50 | Scroll Depth (50%) | article_name, scroll_percentage, page_url | Measure user engagement |
| blog_scroll_90 | Scroll Depth (90%) | article_name, scroll_percentage, page_url | Identify highly engaged readers |
| booking_step_completed | Custom Event | step_number, step_name, clinic_location | Funnel analysis |
| appointment_booking_completed | Form Submit | booking_id, clinic_location, specialty | Primary conversion tracking |

---

# Multi-Step Booking Funnel Tracking

The appointment booking form consists of three steps.

Step 1 → Select Clinic & Specialty

Step 2 → Enter Patient Details

Step 3 → Confirm Booking

Each step pushes a custom event into the dataLayer. These events are captured by Google Tag Manager and forwarded to Google Analytics 4. This allows Funnel Exploration reports to identify where users drop off before completing their booking.

---

# Step 1 - Clinic & Specialty Selection

### GTM Trigger

Custom Event Trigger

Event Name:

booking_step_completed

### dataLayer Push

```json
{
  "event": "booking_step_completed",
  "step_number": 1,
  "step_name": "clinic_specialty_selected",
  "clinic_location": "Bengaluru",
  "specialty": "Orthopaedics"
}
```

---

# Step 2 - Patient Details

### GTM Trigger

Custom Event Trigger

Event Name:

booking_step_completed

### dataLayer Push

```json
{
  "event": "booking_step_completed",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "patient_name": "Hritik Singh",
  "phone_number": "9876543210",
  "clinic_location": "Bengaluru"
}
```

---

# Step 3 - Booking Confirmation

### GTM Trigger

Custom Event Trigger

Event Name:

appointment_booking_completed

### dataLayer Push

```json
{
  "event": "appointment_booking_completed",
  "step_number": 3,
  "booking_status": "Success",
  "clinic_location": "Bengaluru",
  "specialty": "Orthopaedics"
}
```

---

# Funnel Tracking in GA4

The booking funnel in Google Analytics 4 will be configured as follows:

```
Step 1
Clinic & Specialty Selected
        │
        ▼
Step 2
Patient Details Entered
        │
        ▼
Step 3
Booking Confirmed
```

This Funnel Exploration helps identify where users leave the booking process before completing their appointment.

---

# Google Ads Conversion

## Selected Conversion

appointment_booking_completed

### Why this conversion?

Although users may click the Call button, open WhatsApp, or download the patient guide, these actions do not necessarily result in a successful appointment.

The **appointment_booking_completed** event represents a confirmed booking request and therefore provides the highest business value. Importing this event into Google Ads enables campaign optimization toward actual appointment bookings instead of intermediate user interactions.