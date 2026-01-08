# Storage Design

This document outlines the storage approach for the Raspberry Pi Mini SOC project and the reasoning behind it.

The system is designed to start simple while accounting for the write-heavy nature of security logging.

---

## Storage Decision

**Initial setup**
- Operating system and applications stored on SD card
- Logs stored locally on the same SD card
- Log retention kept short

**Planned improvement**
- External USB SSD used for log storage
- OS and applications remain on SD card
- High-write directories migrated to SSD

---

## Log Write Characteristics

This project generates continuous and frequent disk writes.

- Cowrie produces verbose JSON logs for every connection, login attempt, and command
- Brute-force activity can rapidly increase log volume
- Loki writes logs in small, frequent chunks during ingestion

These patterns are typical of SOC-style monitoring but are not ideal for SD card longevity.

---

## SD Card Wear Considerations

SD cards are not designed for sustained write-heavy workloads.

Potential issues include:
- Faster wear due to constant small writes
- Reduced performance over time
- Increased risk of card failure

Using an SD card is acceptable for development and demonstration but not ideal for long-term operation.

---

## SSD Migration Plan

The system is designed so log storage can be moved to an external USB SSD with minimal changes.

Planned migration includes:
- Mounting an SSD at a dedicated location
- Redirecting Loki storage to the SSD
- Moving Cowrie log directories to the SSD

This improves durability and allows longer log retention.

---

## Summary

The initial SD-based design keeps the project simple, while planning for SSD migration reflects realistic SOC operational thinking around log volume, reliability, and scalability.
