# Send Matplotlib Figures via Email (Without Saving to Disk)

A Python utility that generates matplotlib plots, encodes them as Base64 strings in memory, and sends them as inline images in an HTML email — all without writing any files to disk.

## Problem Statement

When automating reports or dashboards, a common need is to email charts and images directly from a Python script. The typical approach involves saving figures to disk and attaching them, which introduces unnecessary I/O and cleanup steps. This project eliminates that by using **in-memory buffers** and **Base64 encoding** to embed images directly into the email body.

## How It Works

1. **Generate plots** using matplotlib and NumPy (e.g., a tangent waveform)
2. **Capture the figure** into an in-memory `BytesIO` buffer instead of saving to a file
3. **Encode to Base64** so the image can be embedded directly in HTML
4. **Build an HTML email** using a Jinja2 template with inline `<img>` tags referencing the Base64 data
5. **Send the email** via Gmail's SMTP server with TLS encryption

### Architecture

```
matplotlib figure
       |
       v
  BytesIO buffer (in-memory)
       |
       v
  Base64-encoded string
       |
       v
  Jinja2 HTML template (inline <img src="data:image/jpeg;base64,...">)
       |
       v
  smtplib (SMTP over TLS) --> Gmail --> Recipient inbox
```

## Tech Stack

| Technology | Purpose |
|---|---|
| **Python** | Core scripting language |
| **matplotlib** | Chart/plot generation |
| **NumPy** | Numerical data for plots |
| **BytesIO** | In-memory binary stream (avoids disk writes) |
| **Base64** | Binary-to-text encoding for image embedding |
| **Jinja2** | HTML email template rendering |
| **smtplib** | SMTP client for sending emails via Gmail |

## Sample Output

The email contains three inline images:

- A **tangent waveform** plot generated at runtime with matplotlib
- A **line plot** (embedded as a pre-encoded Base64 string)
- A **car image** (embedded as a pre-encoded Base64 string)

See [output.pdf](ouput.pdf) for a preview of the received email.

## Setup & Usage

### Prerequisites

```bash
pip install numpy matplotlib jinja2
```

### Configuration

Open `email_trigger_with_base64 encoded_string.py` and update the following:

| Line | Field | Description |
|---|---|---|
| 18 | `msg['From']` | Your Gmail address |
| 20 | `password` | Your Gmail App Password |
| 22 | `msg['To']` | Recipient email address |

> **Note:** Gmail requires an [App Password](https://support.google.com/accounts/answer/185833) if 2-Step Verification is enabled. Do not use your regular account password.

### Run

```bash
python "email_trigger_with_base64 encoded_string.py"
```

On success, the script prints:

```
successfully sent email to <recipient>:
```

## Key Concepts Demonstrated

- **In-memory image processing** — using `BytesIO` to avoid file system dependencies
- **Base64 encoding** — embedding binary data directly in HTML for portable, self-contained emails
- **HTML email composition** — building rich email bodies with Jinja2 templates
- **SMTP with TLS** — secure email delivery using `smtplib` and Gmail's SMTP server

## License

This project is open source and available for personal and educational use.
