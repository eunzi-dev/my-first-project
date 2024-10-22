import React, { useState, useEffect } from 'react';
import { Command, Sparkles, Copy, RefreshCw, Heart } from 'lucide-react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';

const NameGenerator = () => {
  const [category, setCategory] = useState('character');
  const [generatedNames, setGeneratedNames] = useState([]);
  const [favorites, setFavorites] = useState([]);
  const [isGenerating, setIsGenerating] = useState(false);
  const [theme, setTheme] = useState('nature'); // nature, mystic, modern
  
  const categories = [
    { id: 'character', label: 'Character Names', icon: '👤' },
    { id: 'project', label: 'Project Names', icon: '🚀' },
    { id: 'brand', label: 'Brand Names', icon: '✨' },
    { id: 'place', label: 'Place Names', icon: '🌍' }
  ];

  const themes = [
    { id: 'nature', label: 'Nature', className: 'from-green-50 to-white' },
    { id: 'mystic', label: 'Mystic', className: 'from-purple-50 to-white' },
    { id: 'modern', label: 'Modern', className: 'from-blue-50 to-white' }
  ];

  const nameBank = {
    character: {
      nature: ['Luna Starweaver', 'Atlas Stone', 'Echo Rivers', 'Sage Moonlight'],
      mystic: ['Zephyr Shadowmend', 'Nova Dreamweaver', 'Raven Nightwhisper', 'Ash Stormborn'],
      modern: ['Alex Sterling', 'Morgan Chase', 'Jordan Blake', 'Quinn Parker']
    },
    project: {
      nature: ['Emerald Dawn', 'Crystal Vision', 'Verdant Quest', 'Pure Horizon'],
      mystic: ['Astral Forge', 'Mystic Echo', 'Ethereal Edge', 'Phoenix Rise'],
      modern: ['NextLevel', 'PureLogic', 'CoreFusion', 'BlueShift']
    },
    brand: {
      nature: ['Leafwise', 'PureFlow', 'GreenSpirit', 'WhisperBrand'],
      mystic: ['StarForge', 'MoonGlow', 'AetherBound', 'MysticMark'],
      modern: ['Zenith', 'Nucleus', 'Quantum', 'Apex']
    },
    place: {
      nature: ['Willowbrook Vale', 'Mistwater Harbor', 'Sunspire Peak', 'Emerald Grove'],
      mystic: ['Shadowkeep Citadel', 'Starfall Haven', 'Moonweave Woods', 'Crystal Spire'],
      modern: ['Nova District', 'Sterling Heights', 'Azure Bay', 'Vertex Plaza']
    }
  };

  const generateNames = () => {
    setIsGenerating(true);
    const names = nameBank[category][theme];
    const shuffled = [...names].sort(() => Math.random() - 0.5).slice(0, 3);
    
    // Animate names appearing one by one
    setGeneratedNames([]);
    setTimeout(() => setGeneratedNames([shuffled[0]], 200));
    setTimeout(() => setGeneratedNames([shuffled[0], shuffled[1]]), 400);
    setTimeout(() => {
      setGeneratedNames(shuffled);
      setIsGenerating(false);
    }, 600);
  };

  const copyToClipboard = (name) => {
    navigator.clipboard.writeText(name);
  };

  const toggleFavorite = (name) => {
    setFavorites(prev => 
      prev.includes(name) 
        ? prev.filter(n => n !== name)
        : [...prev, name]
    );
  };

  return (
    <div className={`min-h-screen bg-gradient-to-b ${themes.find(t => t.id === theme).className} p-8`}>
      <Card className="max-w-3xl mx-auto bg-white/90 backdrop-blur-sm shadow-lg">
        <CardHeader className="text-center">
          <div className="flex items-center justify-center mb-4">
            <Command className="h-8 w-8 text-green-600" />
          </div>
          <CardTitle className="text-3xl font-light text-green-800">Name Generator</CardTitle>
          <p className="text-green-600 mt-2">Discover unique names for your creative endeavors</p>
        </CardHeader>
        
        <CardContent>
          {/* Theme Selector */}
          <div className="flex justify-center gap-4 mb-6">
            {themes.map(t => (
              <button
                key={t.id}
                onClick={() => setTheme(t.id)}
                className={`px-4 py-2 rounded-full transition-all ${
                  theme === t.id
                    ? 'bg-green-100 text-green-800'
                    : 'bg-white text-green-600 hover:bg-green-50'
                }`}
              >
                {t.label}
              </button>
            ))}
          </div>

          {/* Animated Ribbon */}
          <div className="relative h-2 mb-8">
            <div className="absolute w-full h-full bg-gradient-to-r from-green-200 via-green-300 to-green-200 rounded-full animate-pulse"></div>
          </div>

          {/* Category Selection */}
          <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-8">
            {categories.map(cat => (
              <button
                key={cat.id}
                onClick={() => setCategory(cat.id)}
                className={`p-4 rounded-lg transition-all flex flex-col items-center ${
                  category === cat.id
                    ? 'bg-green-100 text-green-800 shadow-md'
                    : 'bg-white text-green-600 hover:bg-green-50'
                }`}
              >
                <span className="text-2xl mb-2">{cat.icon}</span>
                {cat.label}
              </button>
            ))}
          </div>

          {/* Generate Button */}
          <button
            onClick={generateNames}
            disabled={isGenerating}
            className="w-full py-4 px-6 bg-green-600 text-white rounded-lg shadow-md hover:bg-green-700 transition-colors flex items-center justify-center gap-2"
          >
            {isGenerating ? (
              <RefreshCw className="w-5 h-5 animate-spin" />
            ) : (
              <Sparkles className="w-5 h-5" />
            )}
            Generate Names
          </button>

          {/* Generated Names Display */}
          <div className="mt-8 space-y-4">
            {generatedNames.map((name, index) => (
              <div 
                key={name}
                className="p-6 bg-green-50 rounded-lg flex items-center justify-between animate-fadeIn"
                style={{ animationDelay: `${index * 200}ms` }}
              >
                <h3 className="text-2xl text-green-800 font-light">{name}</h3>
                <div className="flex gap-2">
                  <button
                    onClick={() => toggleFavorite(name)}
                    className={`p-2 rounded-full transition-colors ${
                      favorites.includes(name)
                        ? 'text-red-500 bg-red-50'
                        : 'text-gray-400 hover:text-red-500 hover:bg-red-50'
                    }`}
                  >
                    <Heart className="w-5 h-5" />
                  </button>
                  <button
                    onClick={() => copyToClipboard(name)}
                    className="p-2 rounded-full text-gray-400 hover:text-green-600 hover:bg-green-50 transition-colors"
                  >
                    <Copy className="w-5 h-5" />
                  </button>
                </div>
              </div>
            ))}
          </div>

          {/* Favorites Section */}
          {favorites.length > 0 && (
            <div className="mt-8">
              <h4 className="text-lg font-medium text-green-800 mb-4">Favorites</h4>
              <div className="flex flex-wrap gap-2">
                {favorites.map(name => (
                  <span key={name} className="px-3 py-1 bg-green-100 text-green-700 rounded-full text-sm">
                    {name}
                  </span>
                ))}
              </div>
            </div>
          )}
        </CardContent>
      </Card>
    </div>
  );
};

export default NameGenerator;
