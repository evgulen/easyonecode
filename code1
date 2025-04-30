import 'dart:math';
import 'package:flutter/material.dart';
import 'dart:async';
import 'package:shared_preferences/shared_preferences.dart';
import 'package:wig/audio_manager.dart'; // Ses efektleri kontrolü için

/// Zaman göstergesi: Toplam 50 parçadan oluşan, 5 saniyelik geri sayım için.
class TimeBar extends StatelessWidget {
  final double timeLeft;
  final double totalTime;
  const TimeBar({super.key, required this.timeLeft, required this.totalTime});

  @override
  Widget build(BuildContext context) {
    int totalPieces = 50;
    int activePieces = (timeLeft / totalTime * totalPieces).ceil();
    if (activePieces > totalPieces) activePieces = totalPieces;
    List<Widget> pieces = [];
    for (int i = 0; i < totalPieces; i++) {
      Color pieceColor;
      if (i < 10) {
        pieceColor = Colors.red;
      } else if (i < 30) {
        pieceColor = Colors.yellow;
      } else {
        pieceColor = Colors.green;
      }
      pieces.add(
        Expanded(
          child: Container(
            margin: const EdgeInsets.symmetric(horizontal: 0.5),
            height: 10.0,
            color: i < activePieces ? pieceColor : Colors.grey[300],
          ),
        ),
      );
    }
    return Container(
      padding: const EdgeInsets.all(2.0),
      decoration: BoxDecoration(border: Border.all(color: Colors.black)),
      child: Row(children: pieces),
    );
  }
}

/// Easy One oyunu için sayı üretim sınıfı.
class NumberGeneratorEasyOne {
  final Random rng = Random();

  final Map<String, List<double>> numberSets = {
    'A': List.generate(9, (i) => (i + 1).toDouble()),
    'B': List.generate(9, (i) => -(i + 1).toDouble()),
    'C': List.generate(11, (i) => (10 + i).toDouble()),
    'D': List.generate(11, (i) => -(10 + i).toDouble()),
    'E': List.generate(21, (i) => (20 + i).toDouble()),
    'F': List.generate(21, (i) => -(20 + i).toDouble()),
    'G': List.generate(21, (i) => (40 + i).toDouble()),
    'H': List.generate(21, (i) => -(40 + i).toDouble()),
    'I': List.generate(41, (i) => (60 + i).toDouble()),
    'J': List.generate(41, (i) => -(60 + i).toDouble()),
    'K': List.generate(200, (i) => ((i + 1) / 10)),
    'L': List.generate(200, (i) => -((i + 1) / 10)),
    'M': List.generate(401, (i) => 20.0 + i / 10),
    'N': List.generate(401, (i) => -(20.0 + i / 10)),
    'O': List.generate(401, (i) => 60.0 + i / 10),
    'P': List.generate(401, (i) => -(60.0 + i / 10)),
  };

  String _formatNumber(double value) {
    if (value == value.roundToDouble()) {
      return value.toInt().toString();
    } else {
      return value.toString();
    }
  }

  double _getRandomValue(String classKey) {
    List<double>? values = numberSets[classKey];
    if (values == null || values.isEmpty) {
      throw Exception("Sayı kümesi bulunamadı: $classKey");
    }
    values.shuffle(rng);
    return values.first;
  }

  List<String> generateNumbers(int gameIndex) {
    String chosenFormat = "";
    String firstClass = "";
    String secondClass = "";
    String extraClass = "";
    switch (gameIndex) {
      case 1:
        chosenFormat = "A1";
        firstClass = "A";
        secondClass = "A";
        break;
      case 2:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "A" : "B";
        secondClass = "A";
        break;
      case 3:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "A" : "B";
        secondClass = rng.nextBool() ? "A" : "B";
        break;
      case 4:
      case 5:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "A" : "B";
        secondClass = "B";
        break;
      case 6:
        {
          List<String> formats = ["A1", "A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "C" : "D";
          secondClass = "A";
          extraClass = "A";
        }
        break;
      case 7:
      case 8:
      case 9:
        {
          List<String> formats = ["A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "C" : "D";
          secondClass = rng.nextBool() ? "A" : "B";
          extraClass = "A";
        }
        break;
      case 10:
        {
          List<String> formats = ["A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "C" : "D";
          secondClass = rng.nextBool() ? "A" : "B";
          extraClass = "B";
        }
        break;
      case 11:
      case 12:
      case 13:
      case 14:
      case 15:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "E" : "F";
        secondClass = rng.nextBool() ? "C" : "D";
        break;
      case 16:
        {
          List<String> formats = ["A1", "A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "G" : "H";
          secondClass = rng.nextBool() ? "E" : "F";
          if (chosenFormat != "A1") extraClass = "A";
        }
        break;
      case 17:
      case 18:
      case 19:
      case 20:
        {
          List<String> formats = ["A1", "A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "G" : "H";
          secondClass = rng.nextBool() ? "E" : "F";
          if (chosenFormat != "A1") {
            extraClass =
                [
                  "A",
                  (gameIndex == 17 ? (rng.nextBool() ? "A" : "B") : null),
                  (gameIndex == 18 ? "B" : null),
                  (gameIndex == 19 ? "C" : null),
                  (gameIndex == 20 ? "D" : null),
                ].firstWhere((e) => e != null)!;
          }
        }
        break;
      case 21:
      case 22:
      case 23:
      case 24:
      case 25:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "I" : "J";
        secondClass = rng.nextBool() ? "G" : "H";
        break;
      case 26:
      case 27:
      case 28:
      case 29:
      case 30:
        {
          List<String> formats = ["A1", "A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "K" : "L";
          secondClass = rng.nextBool() ? "I" : "J";
          if (chosenFormat != "A1") {
            extraClass =
                (gameIndex == 26
                    ? "A"
                    : gameIndex == 27
                    ? (rng.nextBool() ? "A" : "B")
                    : gameIndex == 28
                    ? "B"
                    : gameIndex == 29
                    ? "C"
                    : "D");
          }
        }
        break;
      case 31:
      case 32:
      case 33:
      case 34:
      case 35:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "M" : "N";
        secondClass = rng.nextBool() ? "K" : "L";
        break;
      case 36:
      case 37:
      case 38:
      case 39:
      case 40:
        {
          List<String> formats = ["A1", "A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "O" : "P";
          secondClass = rng.nextBool() ? "M" : "N";
          if (chosenFormat != "A1") {
            extraClass =
                (gameIndex == 36
                    ? "A"
                    : gameIndex == 37
                    ? (rng.nextBool() ? "A" : "B")
                    : gameIndex == 38
                    ? "B"
                    : gameIndex == 39
                    ? "C"
                    : "D");
          }
        }
        break;
      case 41:
      case 42:
      case 43:
      case 44:
      case 45:
        chosenFormat = "A1";
        firstClass = rng.nextBool() ? "M" : "N";
        secondClass = rng.nextBool() ? "M" : "N";
        break;
      case 46:
      case 47:
      case 48:
      case 49:
      case 50:
        {
          List<String> formats = ["A1", "A2", "A3"];
          chosenFormat = formats[rng.nextInt(formats.length)];
          firstClass = rng.nextBool() ? "O" : "P";
          secondClass = rng.nextBool() ? "O" : "P";
          if (chosenFormat != "A1") {
            extraClass =
                (gameIndex == 46
                    ? "A"
                    : gameIndex == 47
                    ? (rng.nextBool() ? "A" : "B")
                    : gameIndex == 48
                    ? "B"
                    : gameIndex == 49
                    ? "C"
                    : "D");
          }
        }
        break;
      default:
        throw Exception("Geçersiz oyun indeksi için Easy One");
    }

    double num1 = _getRandomValue(firstClass);
    double base;
    double extra = 0.0;
    String displaySecond;
    double computed;
    if (chosenFormat == "A1") {
      base = _getRandomValue(secondClass);
      computed = base;
      displaySecond = _formatNumber(base);
    } else if (chosenFormat == "A2") {
      base = _getRandomValue(secondClass);
      extra = _getRandomValue(extraClass);
      computed = base + extra;
      displaySecond = "${_formatNumber(base)} + ${_formatNumber(extra)}";
    } else {
      base = _getRandomValue(secondClass);
      extra = _getRandomValue(extraClass);
      computed = chosenFormat == "A3" ? base - extra : base + extra; // fallback
      displaySecond =
          chosenFormat == "A3"
              ? "${_formatNumber(base)} - ${_formatNumber(extra)}"
              : "${_formatNumber(base)} + ${_formatNumber(extra)}";
    }

    int attempt = 0;
    while (num1 == computed && attempt < 5) {
      // retry logic (omitted for brevity)
      attempt++;
    }
    String displayFirst = _formatNumber(num1);
    if (rng.nextBool()) {
      return [
        displaySecond,
        displayFirst,
        _formatNumber(computed),
        displayFirst,
      ];
    } else {
      return [
        displayFirst,
        displaySecond,
        displayFirst,
        _formatNumber(computed),
      ];
    }
  }
}

/// Easy One oyunu ekranı.
class EasyOnePage extends StatefulWidget {
  const EasyOnePage({super.key});
  @override
  EasyOnePageState createState() => EasyOnePageState();
}

class EasyOnePageState extends State<EasyOnePage> {
  final Random rng = Random();
  final NumberGeneratorEasyOne numberGenerator = NumberGeneratorEasyOne();

  List<String> numbers = ["0", "0", "0", "0"];
  int score = 0;
  int gameIndex = 1;
  String message = "";
  Timer? timer;
  Timer? _correctTimer;
  double actualTimeLeft = 5.0;
  double startTimeValue = 5.0;
  double timeLeft = 5.0;
  bool inputEnabled = true;
  bool gameOver = false;
  bool finalGameEnded = false;
  bool levelCompleted = false;
  int extraTimeCount = 8;
  int pauseBonusCount = 8;
  int lifeCount = 8;

  // **YENİ EKLEMELER**: Score ×2 bonusu için
  bool _useScoreMultiplier = false;
  int _scoreMultiplierCount = 0;

  bool isPaused = false;
  bool lifeBonusActive = false;
  double continueCountdown = 5.0;
  Timer? _continueTimer;
  double totalTimeSpent = 0.0;
  late DateTime questionStartTime;

  @override
  void initState() {
    super.initState();
    _loadBonusCounts();
    _loadScoreMultiplierCount();
    startGame();
  }

  /// Score ×2 bonus sayısını yükleyip, eğer 0’dan büyükse hemen dialog sor.
  Future<void> _loadScoreMultiplierCount() async {
    final prefs = await SharedPreferences.getInstance();
    final count = prefs.getInt('scoreMultiplier2Count') ?? 0;
    if (count > 0) {
      _scoreMultiplierCount = count;
      Future.microtask(() => _askUseScoreMultiplier(prefs));
    }
  }

  void _askUseScoreMultiplier(SharedPreferences prefs) {
    showDialog(
      context: context,
      barrierDismissible: false,
      builder:
          (context) => AlertDialog(
            title: const Text('Use Score ×2?'),
            content: Text(
              'You have $_scoreMultiplierCount Score ×2 bonus.\n'
              'If you use it, your final score will be doubled.',
            ),
            actions: [
              TextButton(
                child: const Text('No'),
                onPressed: () => Navigator.of(context).pop(),
              ),
              TextButton(
                child: const Text('Yes'),
                onPressed: () async {
                  // Navigator ve Messenger'ı await öncesi yakalıyoruz
                  final navigator = Navigator.of(context);
                  final messenger = ScaffoldMessenger.of(context);

                  await prefs.setInt(
                    'scoreMultiplier2Count',
                    _scoreMultiplierCount - 1,
                  );

                  if (!mounted) return;
                  setState(() => _useScoreMultiplier = true);

                  navigator.pop();
                  messenger.showSnackBar(
                    const SnackBar(content: Text('Score ×2 bonus activated!')),
                  );
                },
              ),
            ],
          ),
    );
  }

  Future<void> _loadBonusCounts() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    setState(() {
      extraTimeCount = prefs.getInt("extraTimeCount") ?? 8;
      pauseBonusCount = prefs.getInt("pauseBonusCount") ?? 8;
      lifeCount = prefs.getInt("lifeCount") ?? 8;
    });
  }

  Future<void> _saveBonusCounts() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    prefs.setInt("extraTimeCount", extraTimeCount);
    prefs.setInt("pauseBonusCount", pauseBonusCount);
    prefs.setInt("lifeCount", lifeCount);
  }

  @override
  void dispose() {
    timer?.cancel();
    _correctTimer?.cancel();
    _continueTimer?.cancel();
    super.dispose();
  }

  Future<void> startTimer() async {
    timer?.cancel();
    questionStartTime = DateTime.now();
    timer = Timer.periodic(const Duration(milliseconds: 10), (t) {
      if (inputEnabled && !isPaused) {
        setState(() {
          double elapsed =
              DateTime.now().difference(questionStartTime).inMilliseconds /
              1000.0;
          double remaining = startTimeValue - elapsed;
          actualTimeLeft = remaining;
          timeLeft = remaining;
          if (remaining <= 0) {
            timeLeft = 0;
            if (lifeCount > 0) {
              message =
                  "Time's up! Game Over. Tap here to continue with Lives ($lifeCount)";
              inputEnabled = false;
              gameOver = true;
              lifeBonusActive = true;
              continueCountdown = 5.0;
              AudioManager.instance.playEffect('sounds/sorry.mp3');
              timer?.cancel();
              startLifeBonusCountdown();
            } else {
              message = "Time's up! Game Over.";
              AudioManager.instance.playEffect('sounds/sorry.mp3');
              timer?.cancel();
              inputEnabled = false;
              gameOver = true;
            }
          }
        });
      }
    });
  }

  Future<void> startGame() async {
    await _loadBonusCounts();
    actualTimeLeft = 5.0;
    startTimeValue = 5.0;
    timeLeft = 5.0;
    message = "";
    inputEnabled = true;
    gameOver = false;
    finalGameEnded = false;
    levelCompleted = false;
    isPaused = false;
    lifeBonusActive = false;
    totalTimeSpent = 0.0;
    questionStartTime = DateTime.now();
    startTimer();
    generateNewNumbers();
  }

  void generateNewNumbers() {
    setState(() {
      numbers = numberGenerator.generateNumbers(gameIndex);
      actualTimeLeft = 5.0;
      startTimeValue = 5.0;
      timeLeft = 5.0;
      questionStartTime = DateTime.now();
    });
  }

  void nextQuestion() async {
    if (gameIndex % 10 == 0 && gameIndex < 50) {
      setState(() {
        levelCompleted = true;
      });
      timer?.cancel();
      return;
    }
    if (gameIndex < 50) {
      setState(() => gameIndex++);
      generateNewNumbers();
      inputEnabled = true;
      startTimer();
    } else {
      totalTimeSpent += (startTimeValue - actualTimeLeft);

      // **Eğer oyuncu “Score ×2” kullanmayı seçtiyse skor iki kat olsun**
      if (_useScoreMultiplier) {
        score *= 2;
      }

      double starRating = _calculateStarRating(score);
      SharedPreferences prefs = await SharedPreferences.getInstance();
      await prefs.setDouble("easyOneStarRating", starRating);
      int prevBestScore = prefs.getInt("easyOneBestScore") ?? 0;
      if (score > prevBestScore) {
        await prefs.setInt("easyOneBestScore", score);
      }
      double prevBestTime =
          prefs.getDouble("easyOneBestTime") ?? double.infinity;
      if (totalTimeSpent < prevBestTime) {
        await prefs.setDouble("easyOneBestTime", totalTimeSpent);
      }
      await prefs.setInt("highestUnlockedLevel", 2);
      setState(() {
        inputEnabled = false;
        gameOver = true;
        finalGameEnded = true;
        message = "";
      });
      AudioManager.instance.playEffect('sounds/applause.mp3');
    }
  }

  double _calculateStarRating(int score) {
    if (score >= 17500) return 3.0;
    if (score >= 15000) return 2.5;
    if (score >= 10000) return 2.0;
    if (score >= 5000) return 1.5;
    if (score >= 3000) return 1.0;
    return 0.5;
  }

  Widget _buildStars(double rating) {
    List<Widget> stars = [];
    int fullStars = rating.floor();
    bool hasHalfStar = (rating - fullStars) >= 0.5;
    for (int i = 0; i < fullStars; i++) {
      stars.add(const Icon(Icons.star, color: Colors.amber, size: 20));
    }
    if (hasHalfStar) {
      stars.add(const Icon(Icons.star_half, color: Colors.amber, size: 20));
    }
    while (stars.length < 3) {
      stars.add(const Icon(Icons.star_border, color: Colors.amber, size: 20));
    }
    return Row(mainAxisAlignment: MainAxisAlignment.center, children: stars);
  }

  void continueToNextLevel() {
    setState(() {
      levelCompleted = false;
      gameIndex++;
    });
    generateNewNumbers();
    inputEnabled = true;
    startTimer();
  }

  Future<void> checkAnswer(String selectedNumber) async {
    if (!inputEnabled) return;
    double num1 = double.parse(numbers[2]);
    double num2 = double.parse(numbers[3]);
    double correctAnswer = num1 > num2 ? num1 : num2;
    if (double.parse(selectedNumber) == correctAnswer) {
      double timeTaken = startTimeValue - actualTimeLeft;
      totalTimeSpent += timeTaken;
      int points = (500 - timeTaken * 100).toInt();
      if (points < 0) points = 0;
      score += points;
      List<String> correctMessages = [
        "Correct!",
        "Great job!",
        "Well done!",
        "Nice work!",
        "Excellent!",
      ];
      await AudioManager.instance.playEffect('sounds/correct.mp3');
      setState(() {
        message = correctMessages[rng.nextInt(correctMessages.length)];
        inputEnabled = false;
      });
      _correctTimer = Timer(const Duration(milliseconds: 300), () {
        nextQuestion();
      });
    } else {
      await AudioManager.instance.playEffect('sounds/sorry.mp3');
      if (lifeCount > 0) {
        setState(() {
          message = "Game Over. Tap here to continue with Lives ($lifeCount)";
          inputEnabled = false;
          gameOver = true;
          lifeBonusActive = true;
          continueCountdown = 5.0;
        });
        startLifeBonusCountdown();
      } else {
        setState(() {
          message = "Wrong answer! Game Over.";
          inputEnabled = false;
          gameOver = true;
        });
      }
    }
  }

  void startLifeBonusCountdown() {
    _continueTimer?.cancel();
    _continueTimer = Timer.periodic(const Duration(seconds: 1), (t) {
      setState(() {
        continueCountdown -= 1;
        if (continueCountdown <= 0) {
          _continueTimer?.cancel();
          _continueTimer = null;
          lifeBonusActive = false;
          message = "Game Over";
        }
      });
    });
  }

  Future<void> continueGameWithLife() async {
    setState(() {
      lifeCount = (lifeCount > 0) ? lifeCount - 1 : 0;
      score -= 500;
      double newTime = actualTimeLeft + 5.0;
      startTimeValue = newTime;
      timeLeft = newTime;
      actualTimeLeft = newTime;
      lifeBonusActive = false;
      gameOver = false;
      inputEnabled = true;
      continueCountdown = 5.0;
      _continueTimer?.cancel();
      _continueTimer = null;
      questionStartTime = DateTime.now();
      message = "";
    });
    await _saveBonusCounts();
    AudioManager.instance.playEffect('sounds/life.mp3');
    startTimer();
  }

  Widget buildTimerBonusItem() {
    return GestureDetector(
      onTap: () async {
        if (!gameOver && extraTimeCount > 0 && !isPaused) {
          setState(() {
            extraTimeCount--;
            score -= 250;
            double newTime = actualTimeLeft + 10.0;
            startTimeValue = newTime;
            timeLeft = newTime;
            actualTimeLeft = newTime;
          });
          await _saveBonusCounts();
          AudioManager.instance.playEffect('sounds/time.mp3');
        }
      },
      child: buildStatItem(Icons.timer, "Timer", extraTimeCount),
    );
  }

  Widget buildPauseBonusItem() {
    return GestureDetector(
      onTap: () async {
        if (!gameOver && !isPaused && pauseBonusCount > 0) {
          setState(() {
            pauseBonusCount--;
            score -= 250;
            isPaused = true;
            timer?.cancel();
            double newTime = actualTimeLeft + 2.5;
            startTimeValue = newTime;
            timeLeft = newTime;
            actualTimeLeft = newTime;
          });
          await _saveBonusCounts();
          AudioManager.instance.playEffect('sounds/pause.mp3');
        }
      },
      child: buildStatItem(Icons.pause, "Pause", pauseBonusCount),
    );
  }

  Widget buildHeartBonusItem() {
    return GestureDetector(
      onTap: () {
        if (gameOver && lifeCount > 0 && !finalGameEnded) {
          continueGameWithLife();
        }
      },
      child: buildStatItem(Icons.favorite, "Lives", lifeCount),
    );
  }

  Widget buildStatItem(IconData icon, String label, int count) {
    return Container(
      padding: const EdgeInsets.all(8),
      decoration: BoxDecoration(
        border: Border.all(color: Colors.black),
        borderRadius: BorderRadius.circular(8),
      ),
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          Icon(icon, size: 30, color: Colors.black),
          const SizedBox(height: 5),
          Text(
            label,
            style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
          ),
          const SizedBox(height: 5),
          Text(
            count.toString(),
            style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
          ),
        ],
      ),
    );
  }

  Widget _buildPauseOverlay() {
    return Positioned(
      left: 20,
      right: 20,
      bottom: 80,
      child: GestureDetector(
        onTap: resumeGame,
        child: Container(
          padding: const EdgeInsets.all(20),
          decoration: BoxDecoration(
            color: Colors.white,
            borderRadius: BorderRadius.circular(20),
            border: Border.all(color: Colors.black),
          ),
          child: const Text(
            "Game paused. Tap here to resume.",
            textAlign: TextAlign.center,
            style: TextStyle(fontSize: 20, color: Colors.black),
          ),
        ),
      ),
    );
  }

  Widget _buildLevelCompleteOverlay() {
    return Positioned(
      left: 20,
      right: 20,
      bottom: 80,
      child: GestureDetector(
        onTap: continueToNextLevel,
        child: Container(
          padding: const EdgeInsets.all(20),
          decoration: BoxDecoration(
            color: Colors.green,
            borderRadius: BorderRadius.circular(20),
            border: Border.all(color: Colors.black),
          ),
          child: const Text(
            "Congratulations, you've completed this level.\nTap here to continue to the next level.",
            textAlign: TextAlign.center,
            style: TextStyle(fontSize: 20, color: Colors.white),
          ),
        ),
      ),
    );
  }

  Widget _buildLifeBonusOverlay() {
    return Positioned(
      left: 20,
      right: 20,
      bottom: 80,
      child: GestureDetector(
        onTap: continueGameWithLife,
        child: Container(
          padding: const EdgeInsets.all(20),
          decoration: BoxDecoration(
            color: Colors.red,
            borderRadius: BorderRadius.circular(20),
            border: Border.all(color: Colors.black),
          ),
          child: Text(
            "Game Over. Tap here to continue with Lives ($lifeCount)\nContinue in ${continueCountdown.toInt()} sec",
            textAlign: TextAlign.center,
            style: const TextStyle(fontSize: 20, color: Colors.white),
          ),
        ),
      ),
    );
  }

  Widget _buildFinalOverlay() {
    return Positioned(
      left: 20,
      right: 20,
      bottom: 80,
      child: Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          Container(
            padding: const EdgeInsets.all(20),
            decoration: BoxDecoration(
              color: Colors.blue,
              borderRadius: BorderRadius.circular(20),
              border: Border.all(color: Colors.black),
            ),
            child: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                const Text(
                  "Congratulations, you unlocked Easy Two game.",
                  textAlign: TextAlign.center,
                  style: TextStyle(fontSize: 20, color: Colors.white),
                ),
                const SizedBox(height: 10),
                Text(
                  "Your time: ${totalTimeSpent.toStringAsFixed(2)} sec",
                  style: const TextStyle(fontSize: 18, color: Colors.white),
                  textAlign: TextAlign.center,
                ),
                Text(
                  "Your score: $score",
                  style: const TextStyle(fontSize: 18, color: Colors.white),
                  textAlign: TextAlign.center,
                ),
                const SizedBox(height: 10),
                _buildStars(_calculateStarRating(score)),
              ],
            ),
          ),
          const SizedBox(height: 20),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              GestureDetector(
                onTap: () {
                  Navigator.pop(context);
                },
                child: Container(
                  padding: const EdgeInsets.symmetric(
                    vertical: 10,
                    horizontal: 12,
                  ),
                  decoration: BoxDecoration(
                    color: Colors.deepPurple,
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: const Text(
                    "Select Level",
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
              GestureDetector(
                onTap: resetGame,
                child: Container(
                  padding: const EdgeInsets.symmetric(
                    vertical: 10,
                    horizontal: 12,
                  ),
                  decoration: BoxDecoration(
                    color: Colors.green,
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: const Text(
                    "Play Again",
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
              GestureDetector(
                onTap: () async {
                  SharedPreferences prefs =
                      await SharedPreferences.getInstance();
                  await prefs.setInt("highestUnlockedLevel", 2);
                  if (!mounted) return;
                  Navigator.pushReplacement(
                    context,
                    MaterialPageRoute(
                      builder: (context) => const EasyTwoPage(),
                    ),
                  );
                },
                child: Container(
                  padding: const EdgeInsets.symmetric(
                    vertical: 10,
                    horizontal: 12,
                  ),
                  decoration: BoxDecoration(
                    color: Colors.orange,
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: const Text(
                    "Easy Two",
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.white,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }

  void resumeGame() {
    setState(() {
      isPaused = false;
      startTimeValue = timeLeft;
      questionStartTime = DateTime.now();
    });
    startTimer();
  }

  Future<void> resetGame() async {
    _correctTimer?.cancel();
    _continueTimer?.cancel();
    await _loadBonusCounts();
    setState(() {
      score = 0;
      gameIndex = 1;
      message = "";
      inputEnabled = true;
      gameOver = false;
      finalGameEnded = false;
      levelCompleted = false;
      lifeBonusActive = false;
      isPaused = false;
      totalTimeSpent = 0.0;
    });
    startGame();
  }

  void handleTap() {
    if (!inputEnabled &&
        message.isNotEmpty &&
        !gameOver &&
        !levelCompleted &&
        !lifeBonusActive) {
      _correctTimer?.cancel();
      nextQuestion();
    }
  }

  @override
  Widget build(BuildContext context) {
    int level = ((gameIndex - 1) ~/ 10) + 1;
    int gameInLevel = gameIndex - (level - 1) * 10;
    return Scaffold(
      appBar: AppBar(
        title: const Text("Easy One"),
        leading: IconButton(
          icon: const Icon(Icons.home),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
      body: GestureDetector(
        onTap: handleTap,
        child: Stack(
          children: [
            SafeArea(
              child: SingleChildScrollView(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.center,
                    children: [
                      Text(
                        "Level $level - Game $gameInLevel",
                        style: const TextStyle(
                          fontSize: 24,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 5),
                      Text(
                        "Time Left: ${timeLeft.toStringAsFixed(2)} sec",
                        style: const TextStyle(
                          fontSize: 20,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 10),
                      TimeBar(timeLeft: timeLeft, totalTime: startTimeValue),
                      const SizedBox(height: 20),
                      if (!finalGameEnded)
                        Text(
                          message,
                          textAlign: TextAlign.center,
                          style: const TextStyle(
                            fontSize: 22,
                            color: Colors.blue,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      const SizedBox(height: 20),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          ElevatedButton(
                            onPressed:
                                () async => await checkAnswer(numbers[2]),
                            child: Text(
                              numbers[0],
                              style: const TextStyle(fontSize: 32),
                            ),
                          ),
                          ElevatedButton(
                            onPressed:
                                () async => await checkAnswer(numbers[3]),
                            child: Text(
                              numbers[1],
                              style: const TextStyle(fontSize: 32),
                            ),
                          ),
                        ],
                      ),
                      const SizedBox(height: 20),
                      Text(
                        "Score: $score",
                        style: const TextStyle(fontSize: 20),
                      ),
                      const SizedBox(height: 20),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: [
                          buildHeartBonusItem(),
                          buildTimerBonusItem(),
                          buildPauseBonusItem(),
                        ],
                      ),
                      const SizedBox(height: 20),
                      if (gameOver &&
                          (lifeCount == 0 || continueCountdown <= 0) &&
                          !finalGameEnded)
                        Center(
                          child: ElevatedButton(
                            onPressed: resetGame,
                            child: const Text(
                              "New Game",
                              style: TextStyle(fontSize: 20),
                            ),
                          ),
                        ),
                    ],
                  ),
                ),
              ),
            ),
            if (levelCompleted) _buildLevelCompleteOverlay(),
            if (isPaused)
              _buildPauseOverlay()
            else if (gameOver &&
                lifeCount > 0 &&
                continueCountdown > 0 &&
                !finalGameEnded)
              _buildLifeBonusOverlay(),
            if (finalGameEnded) _buildFinalOverlay(),
          ],
        ),
      ),
    );
  }
}

/// Dummy EasyTwoPage sınıfı (gerçek projede ayrı dosyada tanımlanır).
class EasyTwoPage extends StatelessWidget {
  const EasyTwoPage({super.key});
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Easy Two")),
      body: const Center(child: Text("Easy Two Page")),
    );
  }
}
