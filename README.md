import React, { useState, useRef } from 'react';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';

export default function StopwatchApp() {
  const [time, setTime] = useState(0);
  const [isRunning, setIsRunning] = useState(false);
  const [laps, setLaps] = useState([]);
  const timerRef = useRef(null);

  const startStopwatch = () => {
    if (!isRunning) {
      setIsRunning(true);
      timerRef.current = setInterval(() => {
        setTime(prevTime => prevTime + 10);
      }, 10);
    }
  };

  const pauseStopwatch = () => {
    clearInterval(timerRef.current);
    setIsRunning(false);
  };

  const resetStopwatch = () => {
    clearInterval(timerRef.current);
    setTime(0);
    setLaps([]);
    setIsRunning(false);
  };

  const recordLap = () => {
    setLaps(prevLaps => [...prevLaps, time]);
  };

  const formatTime = (milliseconds) => {
    const minutes = Math.floor(milliseconds / 60000);
    const seconds = Math.floor((milliseconds % 60000) / 1000);
    const centiseconds = Math.floor((milliseconds % 1000) / 10);
    return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}:${String(centiseconds).padStart(2, '0')}`;
  };

  return (
    <div className="p-6 max-w-md mx-auto">
      <Card className="shadow-xl">
        <CardContent className="text-center space-y-4">
          <h1 className="text-2xl font-bold">Stopwatch</h1>
          <div className="text-4xl font-mono">{formatTime(time)}</div>
          <div className="space-x-2">
            <Button onClick={startStopwatch} disabled={isRunning}>Start</Button>
            <Button onClick={pauseStopwatch} disabled={!isRunning}>Pause</Button>
            <Button onClick={resetStopwatch}>Reset</Button>
            <Button onClick={recordLap} disabled={!isRunning}>Lap</Button>
          </div>
          <div className="text-left mt-4">
            <h2 className="text-lg font-semibold">Laps</h2>
            <ul className="list-decimal pl-6">
              {laps.map((lap, index) => (
                <li key={index}>{formatTime(lap)}</li>
              ))}
            </ul>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}
