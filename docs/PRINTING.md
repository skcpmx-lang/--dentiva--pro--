# Print and PDF architecture / validation

Status: planned, not yet certified. See acceptance PRINT-01/02/03 and BN-01.

Both documents use isolated local renderers and escape every user-supplied value. Bundle Inter/Noto Sans Bengali and wait for fonts/images before PDF/print. Wide A4/A5 prescription has clinic/dentist credentials and patient header, clinical findings left, medicines right, configurable footer/hours and genuinely blank handwritten signature area. Narrow mini/thermal templates stack content, never scale A4 to illegibility. Invoice has clinic identity, items and deterministic totals/payment state; no doctor signature or prescription footer.

Paper settings: A4, A5, thermal 58/80 mm where Windows driver supports, mini and custom dimensions, valid margins/orientation/copies. Separate prescription/invoice printer profiles and actual preview. Enumerate Windows-exposed printers through Electron; use native print selection and local `printToPDF`/save dialog. Wireless/Bluetooth support means installed Windows printer drivers, not raw radio integration. Never promise compatibility with every device.

Test long English/Bengali names/addresses/multi-credentials, 1/2/20+ medications, 50+ invoice lines, mixed Unicode, page breaks/footer/signature, fonts embedded in local PDF and printed output. Prevent narrow overflow by reflowing totals/item rows. Capture layout screenshots/PDF and actual Windows paper samples; browser HTML preview alone is not print certification. Clinical and financial authorization applies when fetching snapshots and generating/exporting documents.
