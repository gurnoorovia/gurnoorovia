// MoodTrackerScreen.js
import React, { useState } from 'react';
import { View, Text, TextInput, Button, StyleSheet } from 'react-native';
import axios from 'axios';

const MoodTrackerScreen = () => {
    const [mood, setMood] = useState('');
    const [note, setNote] = useState('');

    const handleSubmit = async () => {
        try {
            await axios.post('http://localhost:5000/api/mood', { mood, note });
            alert('Mood logged successfully');
        } catch (error) {
            console.error(error);
            alert('Failed to log mood');
        }
    };

    return (
        <View style={styles.container}>
            <Text>Mood Tracker</Text>
            <TextInput
                style={styles.input}
                placeholder="Rate your mood (1-10)"
                value={mood}
                onChangeText={setMood}
                keyboardType="numeric"
            />
            <TextInput
                style={styles.input}
                placeholder="Additional notes"
                value={note}
                onChangeText={setNote}
            />
            <Button title="Submit" onPress={handleSubmit} />
        </View>
    );
};

const styles = StyleSheet.create({
    container: { padding: 20 },
    input: { borderColor: 'gray', borderWidth: 1, marginBottom: 10, padding: 8 }
});

export default MoodTrackerScreen;
// server.js
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

mongoose.connect('mongodb://localhost:27017/emotranquil', { useNewUrlParser: true, useUnifiedTopology: true });

const MoodSchema = new mongoose.Schema({
    mood: Number,
    note: String,
    date: { type: Date, default: Date.now }
});
const Mood = mongoose.model('Mood', MoodSchema);

app.post('/api/mood', async (req, res) => {
    try {
        const mood = new Mood(req.body);
        await mood.save();
        res.status(201).send(mood);
    } catch (error) {
        res.status(400).send(error);
    }
});

app.listen(5000, () => console.log('Server is running on port 5000'));
// EmotionLibraryScreen.js
import React, { useState, useEffect } from 'react';
import { View, Text, Button, FlatList, StyleSheet } from 'react-native';
import axios from 'axios';

const EmotionLibraryScreen = () => {
    const [emotions, setEmotions] = useState([]);

    useEffect(() => {
        const fetchEmotions = async () => {
            try {
                const response = await axios.get('http://localhost:5000/api/emotions');
                setEmotions(response.data);
            } catch (error) {
                console.error(error);
            }
        };

        fetchEmotions();
    }, []);

    return (
        <View style={styles.container}>
            <Text>Emotion Library</Text>
            <FlatList
                data={emotions}
                keyExtractor={(item) => item._id}
                renderItem={({ item }) => (
                    <View style={styles.item}>
                        <Text style={styles.title}>{item.name}</Text>
                        <Text>{item.description}</Text>
                    </View>
                )}
            />
        </View>
    );
};

const styles = StyleSheet.create({
    container: { padding: 20 },
    item: { marginBottom: 15 },
    title: { fontWeight: 'bold' }
});

export default EmotionLibraryScreen;
// server.js (add to existing code)
const EmotionSchema = new mongoose.Schema({
    name: String,
    description: String
});
const Emotion = mongoose.model('Emotion', EmotionSchema);

app.get('/api/emotions', async (req, res) => {
    try {
        const emotions = await Emotion.find();
        res.status(200).send(emotions);
    } catch (error) {
        res.status(400).send(error);
    }
});
// BreathingExercisesScreen.js
import React from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

const BreathingExercisesScreen = () => {
    const handleExercise = (type) => {
        alert(`Start ${type} exercise`);
    };

    return (
        <View style={styles.container}>
            <Text>Breathing Exercises</Text>
            <Button title="Deep Breathing" onPress={() => handleExercise('Deep Breathing')} />
            <Button title="Box Breathing" onPress={() => handleExercise('Box Breathing')} />
            <Button title="4-7-8 Breathing" onPress={() => handleExercise('4-7-8 Breathing')} />
        </View>
    );
};

const styles = StyleSheet.create({
    container: { padding: 20 }
});

export default BreathingExercisesScreen;
// server.js (add to existing code)
app.post('/api/coping-strategies', async (req, res) => {
    // This is a placeholder. In practice, you would use AI/ML to provide personalized recommendations.
    const strategies = [
        'Practice mindfulness',
        'Go for a walk',
        'Try deep breathing exercises'
    ];
    res.status(200).send({ strategies });
});
// CopingStrategiesScreen.js
import React, { useState } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';
import axios from 'axios';

const CopingStrategiesScreen = () => {
    const [strategies, setStrategies] = useState([]);

    const fetchStrategies = async () => {
        try {
            const response = await axios.post('http://localhost:5000/api/coping-strategies');
            setStrategies(response.data.strategies);
        } catch (error) {
            console.error(error);
        }
    };

    return (
        <View style={styles.container}>
            <Text>Coping Strategies</Text>
            <Button title="Get Strategies" onPress={fetchStrategies} />
            {strategies.map((strategy, index) => (
                <Text key={index}>{strategy}</Text>
            ))}
        </View>
    );
};

const styles = StyleSheet.create({
    container: { padding: 20 }
});

export default CopingStrategiesScreen;
// EmotionArtScreen.js
import React from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

const EmotionArtScreen = () => {
    const handleArtCreation = () => {
        alert('Start creating emotion-driven art');
    };

    return (
        <View style={styles.container}>
            <Text>Emotion-Driven Art</Text>
            <Button title="Create Art" onPress={handleArtCreation} />
        </View>
    );
};

const styles = StyleSheet.create({
    container: { padding: 20 }
});

export default EmotionArtScreen;
// ProgressInsightsScreen.js
import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet } from 'react-native';
import axios from 'axios';

const ProgressInsightsScreen = () => {
    const [progress, setProgress] = useState({});

    useEffect(() => {
        const fetchProgress = async () => {
            try {
                const response = await axios.get('http://localhost:5000/api/progress');
                setProgress(response.data);
            } catch (error) {
                console.error(error);
            }
        };

        fetchProgress();
    }, []);

    return (
        <View style={styles.container}>
            <Text>Progress Insights</Text>
            <Text>Emotional Growth: {progress.growth}</Text>
            <Text>Goals Progress: {progress.goals}</Text>
        </View>
    );
};

const styles = StyleSheet.create({
    container: { padding: 20 }
});

export default ProgressInsightsScreen;
// server.js (add to existing code)
app.get('/api/progress', async (req, res) => {
    // Placeholder data
    const progress = {
        growth: 'Positive trend',
        goals: '50% completed'
   

<!---
gurnoorovia/gurnoorovia is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
