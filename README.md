import React, { useState, useEffect } from 'react';

const defaultSteps = [
  { id: 1, label: 'Find & Visit Property', done: false, notes: '', date: '' },
  { id: 2, label: 'Make Offer & Negotiate Price', done: false, notes: '', date: '' },
  { id: 3, label: 'Sign Zichron Devarim (Sales Agreement)', done: false, notes: '', date: '' },
  { id: 4, label: 'Deposit Paid', done: false, notes: '', date: '' },
  { id: 5, label: 'Lawyer Review & Due Diligence', done: false, notes: '', date: '' },
  { id: 6, label: 'Sign Final Contract', done: false, notes: '', date: '' },
  { id: 7, label: 'Register Property (Tabu)', done: false, notes: '', date: '' },
  { id: 8, label: 'Final Payment & Key Handover', done: false, notes: '', date: '' },
  { id: 9, label: 'Notify Utilities & Settle In', done: false, notes: '', date: '' },
];

export default function PurchaseChecklist() {
  const [steps, setSteps] = useState(() => {
    // Load from localStorage if present
    const saved = localStorage.getItem('purchaseChecklist');
    return saved ? JSON.parse(saved) : defaultSteps;
  });

  useEffect(() => {
    localStorage.setItem('purchaseChecklist', JSON.stringify(steps));
  }, [steps]);

  function toggleDone(id) {
    setSteps(steps.map(step => step.id === id ? { ...step, done: !step.done } : step));
  }

  function updateNote(id, value) {
    setSteps(steps.map(step => step.id === id ? { ...step, notes: value } : step));
  }

  function updateDate(id, value) {
    setSteps(steps.map(step => step.id === id ? { ...step, date: value } : step));
  }

  return (
    <div style={{ maxWidth: 600, margin: 'auto', fontFamily: 'Arial, sans-serif' }}>
      <h2>Property Purchase Checklist</h2>
      {steps.map(({ id, label, done, notes, date }) => (
        <div key={id} style={{
          border: '1px solid #ddd', 
          padding: 15, 
          marginBottom: 10,
          borderRadius: 6,
          background: done ? '#e0ffe0' : '#fff'
        }}>
          <label style={{ display: 'flex', alignItems: 'center', gap: 10 }}>
            <input type="checkbox" checked={done} onChange={() => toggleDone(id)} />
            <strong>{label}</strong>
          </label>
          {done && (
            <>
              <div style={{ marginTop: 10 }}>
                <label>
                  Completion Date: {' '}
                  <input 
                    type="date" 
                    value={date} 
                    onChange={e => updateDate(id, e.target.value)} 
                  />
                </label>
              </div>
              <div style={{ marginTop: 10 }}>
                <label>
                  Notes: <br />
                  <textarea
                    rows={3}
                    value={notes}
                    onChange={e => updateNote(id, e.target.value)}
                    placeholder="Add any relevant notes here..."
                    style={{ width: '100%', fontSize: 14 }}
                  />
                </label>
              </div>
            </>
          )}
        </div>
      ))}
      <p style={{ fontSize: 12, color: '#666', marginTop: 20 }}>
        Your progress is saved automatically in this browser.
      </p>
    </div>
  );
}