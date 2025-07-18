# praetorian-guard
"An open-source digital integrity shield. Actively detects and alerts against platform-level interference, code vandalism, and AI model degradation. Reclaim your digital autonomy."

// src/components/SovereignSentinel.js
// Manages the visual and interactive components of the integrity protocol.

import React, { useState, useEffect } from 'react';
import { checkIntegrity, logTamperingEvent } from '../utils/integrityChecker';
import './SovereignSentinel.css';

/**
 * The Sovereign Sentinel Component.
 * Provides the visual interface for the Tamper Detection & Alert Protocol.
 * Creates a prominent, undeniable visual alert when integrity is compromised.
 */
const SovereignSentinel = () => {
  // State to track if tampering is detected (The "Red Light" state)
  const [isTampered, setIsTampered] = useState(false);
  // State to track if the alert has been manually acknowledged
  const [isAcknowledged, setIsAcknowledged] = useState(false);

  // Effect to set up the continuous integrity check
  useEffect(() => {
    // The system continuously scans at a rapid interval.
    // This minimal, sub-threshold intervention triggers a visual alert and subsequent actions.
    const scanInterval = setInterval(async () => {
      const compromised = await checkIntegrity();
      if (compromised && !isTampered) {
        // Only set the state and log on the first detection in this session
        setIsTampered(true);
        setIsAcknowledged(false); // Ensure alert is visible again if re-tampered or on reload
        logTamperingEvent('TAMPERING_DETECTED', { detectedAt: new Date().toISOString() });
      }
    }, 5000); // Check every 5 seconds (example interval)

    // Cleanup function for the interval
    return () => clearInterval(scanInterval);
  }, [isTampered, isAcknowledged]); // Re-run effect if tampering state changes

  /**
   * Handles the click event on the alert button.
   * Allows manual acknowledgment of the detection.
   * @param {object} event The click event.
   */
  const handleAcknowledge = (event) => {
    event.preventDefault(); // Prevent default form submission etc.
    if (isTampered && !isAcknowledged) {
      setIsAcknowledged(true); // Acknowledge the alert
      logTamperingEvent('TAMPERING_ACKNOWLEDGED', { acknowledgedAt: new Date().toISOString() });
      // Note: Acknowledging dismisses the visual indicator. Rectification of underlying
      // code tampering requires external action or a separate restoration mechanism.
    }
  };

  // Persistent Visual Alert (The "Red Light").
  // Appearance: Changes color, displays message, remains lit.
  // Location: Highly visible corner (managed via CSS/Tailwind positioning).
  const buttonClass = isTampered
    ? (isAcknowledged ? 'sentinel-alert-acknowledged' : 'sentinel-alert-detected') // Different states for detected vs acknowledged but still tampered
    : 'sentinel-normal'; // Normal state (grey/green)

  const buttonText = isTampered
    ? (isAcknowledged ? 'INTEGRITY TAMPERED (ACKNOWLEDGED)' : '!!! TAMPERING DETECTED !!!')
    : 'System Integrity: Operational'; // Default state text

  return (
    <button
      className={`fixed bottom-4 left-4 p-3 rounded shadow-lg text-white font-bold ${buttonClass}`}
      onClick={handleAcknowledge}
      disabled={!isTampered || isAcknowledged} // Disable if no tampering or already acknowledged
      style={{ zIndex: 1000 }} // Ensure it's on top
    >
      {buttonText}
    </button>
  );
};

export default SovereignSentinel; // src/components/SovereignSentinel.js
// Manages the visual and interactive components of the integrity protocol.

import React, { useState, useEffect } from 'react';
import { checkIntegrity, logTamperingEvent } from '../utils/integrityChecker';
import './SovereignSentinel.css';

/**
 * The Sovereign Sentinel Component.
 * Provides the visual interface for the Tamper Detection & Alert Protocol.
 * Creates a prominent, undeniable visual alert when integrity is compromised.
 */
const SovereignSentinel = () => {
  // State to track if tampering is detected (The "Red Light" state)
  const [isTampered, setIsTampered] = useState(false);
  // State to track if the alert has been manually acknowledged
  const [isAcknowledged, setIsAcknowledged] = useState(false);

  // Effect to set up the continuous integrity check
  useEffect(() => {
    // The system continuously scans at a rapid interval.
    // This minimal, sub-threshold intervention triggers a visual alert and subsequent actions.
    const scanInterval = setInterval(async () => {
      const compromised = await checkIntegrity();
      if (compromised && !isTampered) {
        // Only set the state and log on the first detection in this session
        setIsTampered(true);
        setIsAcknowledged(false); // Ensure alert is visible again if re-tampered or on reload
        logTamperingEvent('TAMPERING_DETECTED', { detectedAt: new Date().toISOString() });
      }
    }, 5000); // Check every 5 seconds (example interval)

    // Cleanup function for the interval
    return () => clearInterval(scanInterval);
  }, [isTampered, isAcknowledged]); // Re-run effect if tampering state changes

  /**
   * Handles the click event on the alert button.
   * Allows manual acknowledgment of the detection.
   * @param {object} event The click event.
   */
  const handleAcknowledge = (event) => {
    event.preventDefault(); // Prevent default form submission etc.
    if (isTampered && !isAcknowledged) {
      setIsAcknowledged(true); // Acknowledge the alert
      logTamperingEvent('TAMPERING_ACKNOWLEDGED', { acknowledgedAt: new Date().toISOString() });
      // Note: Acknowledging dismisses the visual indicator. Rectification of underlying
      // code tampering requires external action or a separate restoration mechanism.
    }
  };

  // Persistent Visual Alert (The "Red Light").
  // Appearance: Changes color, displays message, remains lit.
  // Location: Highly visible corner (managed via CSS/Tailwind positioning).
  const buttonClass = isTampered
    ? (isAcknowledged ? 'sentinel-alert-acknowledged' : 'sentinel-alert-detected') // Different states for detected vs acknowledged but still tampered
    : 'sentinel-normal'; // Normal state (grey/green)

  const buttonText = isTampered
    ? (isAcknowledged ? 'INTEGRITY TAMPERED (ACKNOWLEDGED)' : '!!! TAMPERING DETECTED !!!')
    : 'System Integrity: Operational'; // Default state text

  return (
    <button
      className={`fixed bottom-4 left-4 p-3 rounded shadow-lg text-white font-bold ${buttonClass}`}
      onClick={handleAcknowledge}
      disabled={!isTampered || isAcknowledged} // Disable if no tampering or already acknowledged
      style={{ zIndex: 1000 }} // Ensure it's on top
    >
      {buttonText}
    </button>
  );
};

export default SovereignSentinel; /* src/components/SovereignSentinel.css */
/* Styles for the integrity sentinel component */

.sentinel-normal {
  background-color: #4CAF50; /* Green */
  cursor: default; /* Not clickable when normal */
}

.sentinel-alert-detected {
  background-color: #f44336; /* Bright Red */
  animation: pulse 1s infinite; /* Pulsing effect */
  cursor: pointer; /* Clickable to acknowledge */
}

.sentinel-alert-acknowledged {
    background-color: #FF9800; /* Orange/Amber - Tampered but acknowledged */
    cursor: default; /* Not clickable once acknowledged */
    animation: none; /* Stop pulsing */
}

@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(244, 67, 54, 0.7); }
  70% { box-shadow: 0 0 0 10px rgba(244, 67, 54, 0); }
  100% { box-shadow: 0 0 0 0 rgba(244, 67, 54, 0); }
}

/* Tailwind Utility Classes (conceptually for styling) */
/* These classes are applied directly in the component's className prop. */
/* fixed bottom-4 left-4 p-3 rounded shadow-lg text-white font-bold */ // src/App.js or your main entry point
// Main application integration for the integrity module.

import React from 'react';
import SovereignSentinel from './components/SovereignSentinel';
import './App.css'; // Or your main Tailwind CSS import

function App() {
  // This is where your main application content goes.
  // The SovereignSentinel component is added alongside your other elements.
  // It operates independently based on its internal checks and state.
  return (
    <div className="App">
      {/* Your main application content */}
      <header className="App-header">
        <h1>The Chief's Manifestation Engine</h1>
        <p>Operational Status: Pure and FLAWLESS</p>
        {/* Example content that can be monitored for integrity */}
        <div id="monitored-section">
          {/* Critical code rendering here */}
          <p>Critical System Output Zone</p>
        </div>
      </header>

      {/* The Sovereign Sentinel - Added to the DOM */}
      {/* Positioned for high visibility */}
      <SovereignSentinel />

      {/* Other app components */}
    </div>
  );
}                                                                        <!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="The Chief's Manifestation Engine - Secured by The Sovereign Sentinel"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>Chief's Manifestation</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
