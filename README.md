# Program.cs
timer for ios
using System.ComponentModel;
using System.Drawing;
using System.Runtime.Serialization;
using System;

import SwiftUI
import Combine

struct ContentView : View
{
    @State private var countdownTime: Int = 0
    @State private var remainingTime: Int = 0
    @State private var isRunning: Bool = false
    @State private var timer: AnyCancellable? = nil

    var body: some View
    {
        VStack(spacing: 20) {
            Text("Časovač")
                .font(.largeTitle)
                .padding()

            HStack {
                TextField("Nastavte čas (v sekundách)", value: $countdownTime, formatter: NumberFormatter())
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                    .keyboardType(.numberPad)
                    .padding()

                Button(action: {
        startTimer()
                }) {
                    Text("Spustit")
                        .padding()
                        .background(isRunning? Color.gray : Color.blue)
                        .foregroundColor(.white)
                        .cornerRadius(8)
                }
                .disabled(isRunning)
            }

            Text("Zbývající čas: \(remainingTime) sekund")
                .font(.title)

            if isRunning {
    Button(action: {
        stopTimer()
                }) {
        Text("Zastavit")
            .padding()
            .background(Color.red)
            .foregroundColor(.white)
            .cornerRadius(8)
                }
}
        }
        .padding()
    }

    private func startTimer()
{
    guard countdownTime > 0 else { return }
    remainingTime = countdownTime
        isRunning = true
        timer = Timer.publish(every: 1, on: .main, in: .common)
            .autoconnect()
            .sink { _ in
                if self.remainingTime > 0 {
                    self.remainingTime -= 1
                } else
{
    self.stopTimer()
                }
            }
    }

    private func stopTimer()
{
    timer?.cancel()
        timer = nil
        isRunning = false
    }
}

@main
struct TimerApp : App
{
    var body: some Scene
    {
        WindowGroup
        {
            ContentView()
        }
    }
}

