import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Trash2 } from 'lucide-react';

const seductionTypes = {
  A: 'The Siren – Uses physical presence and sensuality to attract.',
  B: 'The Rake – Intensely passionate, focuses attention and desire on the target.',
  C: 'The Ideal Lover – Offers fantasy and fulfills deep emotional needs.',
  D: 'The Dandy – Creates mystery and freedom from traditional roles.',
  E: 'The Natural – Childlike and playful, evokes joy and spontaneity.',
  F: 'The Coquette – Hot and cold, draws people in with withholding.',
  G: 'The Charmer – Smooth, warm, and socially adept, wins with grace.',
  H: 'The Charismatic – Confident, visionary, and magnetic personality.'
};

const questions = [
  {
    text: '1. What kind of vibe or energy do you naturally give off?',
    options: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
  },
  {
    text: '2. Do you enjoy being chased or chasing someone else?',
    options: ['B', 'F', 'H']
  },
  {
    text: '3. Which of these best fits your flirting style?',
    options: ['C', 'E', 'F', 'D']
  },
  {
    text: '4. What elements do you use most to attract people?',
    options: ['A', 'C', 'F', 'H']
  },
  {
    text: '5. Which traits do people often compliment you for?',
    options: ['E', 'G', 'B', 'D']
  }
];

export default function SeductionChatbot() {
  const [answers, setAnswers] = useState({});
  const [history, setHistory] = useState([]);
  const [result, setResult] = useState(null);

  const handleAnswer = (qIndex, option) => {
    setAnswers({ ...answers, [qIndex]: [...(answers[qIndex] || []), option] });
  };

  const calculateResult = () => {
    const count = {};
    Object.values(answers).flat().forEach((ans) => {
      count[ans] = (count[ans] || 0) + 1;
    });
    const sorted = Object.entries(count).sort((a, b) => b[1] - a[1]);
    const topType = sorted[0]?.[0];
    const brief = seductionTypes[topType];
    const currentResult = { answers, type: topType, description: brief };
    const updatedHistory = [currentResult, ...history].slice(0, 20);
    setHistory(updatedHistory);
    setResult(currentResult);
  };

  const reset = () => {
    setAnswers({});
    setResult(null);
  };

  const deleteHistoryItem = (index) => {
    const updated = history.filter((_, i) => i !== index);
    setHistory(updated);
  };

  return (
    <div className="bg-black text-white min-h-screen p-6 space-y-6">
      <h1 className="text-3xl font-bold">Seduction Type Detector</h1>
      {!result ? (
        <div className="space-y-6">
          {questions.map((q, idx) => (
            <Card key={idx} className="bg-gray-900">
              <CardContent className="p-4">
                <p className="mb-2">{q.text}</p>
                <div className="flex flex-wrap gap-2">
                  {q.options.map((opt) => (
                    <Button
                      key={opt}
                      variant={answers[idx]?.includes(opt) ? 'default' : 'outline'}
                      onClick={() => handleAnswer(idx, opt)}
                    >
                      {opt}
                    </Button>
                  ))}
                </div>
              </CardContent>
            </Card>
          ))}
          <Button className="bg-blue-500 hover:bg-blue-600" onClick={calculateResult}>
            Submit
          </Button>
        </div>
      ) : (
        <div className="space-y-4">
          <h2 className="text-xl font-semibold">Your Seduction Type: {result.type}</h2>
          <p>{result.description}</p>
          <Button onClick={reset} className="bg-gray-700 hover:bg-gray-600">Try Again</Button>
        </div>
      )}

      {history.length > 0 && (
        <div className="mt-8">
          <h2 className="text-2xl font-bold mb-4">History</h2>
          <div className="space-y-4">
            {history.map((item, idx) => (
              <Card key={idx} className="bg-gray-800">
                <CardContent className="p-4">
                  <div className="flex justify-between items-center">
                    <div>
                      <p className="font-semibold">Result: {item.type}</p>
                      <p className="text-sm text-gray-300">{item.description}</p>
                      <p className="text-sm text-gray-400 mt-2">
                        Answers: {Object.values(item.answers).flat().join(', ')}
                      </p>
                    </div>
                    <Button
                      variant="ghost"
                      size="icon"
                      onClick={() => deleteHistoryItem(idx)}
                    >
                      <Trash2 className="text-red-500" />
                    </Button>
                  </div>
                </CardContent>
              </Card>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}


Your Seduction Type chatbot is ready! It includes:

All 5 questions at once

Multi-option selection using A–H

Brief descriptions of each seduction type

Dark theme interface

Result analyzer that shows the most frequent seduction type

History panel with up to 20 saved results

Delete button for individual history entries


Let me know if you want it deployed or need enhancements like animations, saving to local storage, or export functionality.

