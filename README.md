# launching-soon


import React, { useState, useEffect } from 'react';
import { Search, Star, ThumbsUp, ThumbsDown, X, User, Calendar, MessageSquare, Award, Send, Plus } from 'lucide-react';

const initialHandymen = [
  {
    id: 1,
    name: 'John Doe',
    skills: ['Plumbing', 'Electrical'],
    rating: 4.5,
    experience: 5,
    likes: 24,
    dislikes: 2,
    reviews: [
      {
        text: 'Great work on my bathroom renovation!',
        reviewer: 'Sarah Johnson',
        date: '2024-12-15'
      },
      {
        text: 'Very professional and punctual',
        reviewer: 'Mike Williams',
        date: '2024-12-10'
      }
    ]
  },
  {
    id: 2,
    name: 'Jane Doe',
    skills: ['Carpentry', 'Painting'],
    rating: 4.2,
    experience: 3,
    likes: 18,
    dislikes: 3,
    reviews: [
      {
        text: 'Excellent attention to detail',
        reviewer: 'Tom Brown',
        date: '2024-12-14'
      },
      {
        text: 'Made my old furniture look brand new',
        reviewer: 'Lisa Anderson',
        date: '2024-12-08'
      }
    ]
  },
  {
    id: 3,
    name: 'Bob Smith',
    skills: ['Plumbing', 'Carpentry'],
    rating: 4.8,
    experience: 10,
    likes: 45,
    dislikes: 1,
    reviews: [
      {
        text: 'Fixed a complex plumbing issue quickly',
        reviewer: 'Chris Davis',
        date: '2024-12-13'
      },
      {
        text: 'Outstanding craftsmanship',
        reviewer: 'Emma Wilson',
        date: '2024-12-05'
      }
    ]
  }
];

const ReviewForm = ({ onSubmit, onCancel }) => {
  const [reviewText, setReviewText] = useState('');
  const [reviewerName, setReviewerName] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (reviewText.trim() && reviewerName.trim()) {
      onSubmit({
        text: reviewText.trim(),
        reviewer: reviewerName.trim(),
        date: new Date().toISOString().split('T')[0]
      });
      setReviewText('');
      setReviewerName('');
    }
  };

  return (
    <form onSubmit={handleSubmit} className="bg-blue-50 p-4 rounded-lg mt-4">
      <div className="mb-4">
        <label className="block text-sm font-medium text-gray-700 mb-1">Your Name</label>
        <input
          type="text"
          value={reviewerName}
          onChange={(e) => setReviewerName(e.target.value)}
          placeholder="Enter your name"
          className="w-full p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
          required
        />
      </div>
      <div className="relative mb-4">
        <label className="block text-sm font-medium text-gray-700 mb-1">Your Review</label>
        <textarea
          value={reviewText}
          onChange={(e) => setReviewText(e.target.value)}
          placeholder="Write your review..."
          className="w-full p-3 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400 min-h-24"
          maxLength={200}
          required
        />
        <div className="absolute bottom-3 right-3 text-sm text-gray-500">
          {reviewText.length}/200
        </div>
      </div>
      <div className="flex gap-2">
        <button
          type="submit"
          className="bg-blue-600 text-white px-4 py-2 rounded-lg flex items-center gap-2 hover:bg-blue-700"
        >
          <Send size={16} />
          Submit Review
        </button>
        <button
          type="button"
          onClick={onCancel}
          className="bg-gray-200 text-gray-600 px-4 py-2 rounded-lg hover:bg-gray-300"
        >
          Cancel
        </button>
      </div>
    </form>
  );
};

const HandymanModal = ({ handyman, onClose, onAddReview, hasUserReviewed }) => {
  const [showReviewForm, setShowReviewForm] = useState(false);

  if (!handyman) return null;

  const handleSubmitReview = (reviewData) => {
    onAddReview(handyman.id, reviewData);
    setShowReviewForm(false);
  };

  return (
    <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50 overflow-y-auto">
      <div className="bg-white rounded-lg max-w-2xl w-full p-6 relative my-8">
        <button
          onClick={onClose}
          className="absolute right-4 top-4 text-gray-500 hover:text-gray-700"
        >
          <X size={24} />
        </button>
        
        <div className="flex flex-col">
          <div className="flex items-center mb-6">
            <div className="bg-blue-100 p-4 rounded-full">
              <User size={48} className="text-blue-600" />
            </div>
            <div className="ml-4">
              <h2 className="text-2xl font-bold text-gray-800">{handyman.name}</h2>
              <div className="flex items-center mt-1">
                <Star className="text-yellow-400" size={20} />
                <span className="ml-1 text-gray-600">{handyman.rating}/5</span>
              </div>
            </div>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
            <div className="bg-purple-50 p-4 rounded-lg">
              <div className="flex items-center mb-2">
                <Award className="text-purple-600" size={20} />
                <h3 className="text-lg font-semibold ml-2 text-purple-700">Skills</h3>
              </div>
              <div className="flex flex-wrap gap-2">
                {handyman.skills.map((skill, index) => (
                  <span
                    key={index}
                    className="bg-purple-200 text-purple-800 px-3 py-1 rounded-full text-sm"
                  >
                    {skill}
                  </span>
                ))}
              </div>
            </div>

            <div className="bg-green-50 p-4 rounded-lg">
              <div className="flex items-center mb-2">
                <Calendar className="text-green-600" size={20} />
                <h3 className="text-lg font-semibold ml-2 text-green-700">Experience</h3>
              </div>
              <p className="text-green-800">
                {handyman.experience} years in the field
              </p>
            </div>
          </div>

          <div className="bg-orange-50 p-4 rounded-lg mb-6">
            <div className="flex items-center justify-between mb-4">
              <div className="flex items-center">
                <MessageSquare className="text-orange-600" size={20} />
                <h3 className="text-lg font-semibold ml-2 text-orange-700">Reviews</h3>
              </div>
              {!hasUserReviewed && !showReviewForm && (
                <button
                  onClick={() => setShowReviewForm(true)}
                  className="flex items-center gap-2 bg-orange-200 text-orange-700 px-3 py-1 rounded-full text-sm hover:bg-orange-300"
                >
                  <Plus size={16} />
                  Add Review
                </button>
              )}
            </div>
            
            {showReviewForm ? (
              <ReviewForm
                onSubmit={handleSubmitReview}
                onCancel={() => setShowReviewForm(false)}
              />
            ) : (
              <div className="space-y-4">
                {handyman.reviews.map((review, index) => (
                  <div key={index} className="bg-white p-4 rounded-lg shadow-sm">
                    <div className="flex justify-between items-start mb-2">
                      <div>
                        <p className="font-medium text-gray-800">{review.reviewer}</p>
                        <p className="text-sm text-gray-500">{review.date}</p>
                      </div>
                    </div>
                    <p className="text-gray-700">{review.text}</p>
                  </div>
                ))}
              </div>
            )}
          </div>

          <div className="flex justify-center gap-8 mt-4">
            <div className="text-center">
              <p className="text-sm text-gray-600 mb-1">Likes</p>
              <p className="text-2xl font-bold text-green-600">{handyman.likes}</p>
            </div>
            <div className="text-center">
              <p className="text-sm text-gray-600 mb-1">Dislikes</p>
              <p className="text-2xl font-bold text-red-600">{handyman.dislikes}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

const App = () => {
  const [handymen, setHandymen] = useState(() => {
    const storedHandymen = localStorage.getItem('handymen');
    return storedHandymen ? JSON.parse(storedHandymen) : initialHandymen;
  });
  const [filteredHandymen, setFilteredHandymen] = useState(handymen);
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedSkill, setSelectedSkill] = useState('');
  const [selectedHandyman, setSelectedHandyman] = useState(null);
  const [userReviews, setUserReviews] = useState(() => {
    const storedReviews = localStorage.getItem('userReviews');
    return storedReviews ? JSON.parse(storedReviews) : [];
  });
  const [likedHandymen, setLikedHandymen] = useState(() => {
    const storedLikes = localStorage.getItem('likedHandymen');
    return storedLikes ? JSON.parse(storedLikes) : [];
  });
  const [dislikedHandymen, setDislikedHandymen] = useState(() => {
    const storedDislikes = localStorage.getItem('dislikedHandymen');
    return storedDislikes ? JSON.parse(storedDislikes) : [];
  });

  useEffect(() => {
    const filteredHandymen = handymen.filter((handyman) => {
      const matchesSearchQuery = handyman.name.toLowerCase().includes(searchQuery.toLowerCase());
      const matchesSelectedSkill = selectedSkill === '' || handyman.skills.includes(selectedSkill);
      return matchesSearchQuery && matchesSelectedSkill;
    });
    setFilteredHandymen(filteredHandymen);
  }, [searchQuery, selectedSkill, handymen]);

  const handleAddReview = (handymanId, reviewData) => {
    if (!userReviews.includes(handymanId)) {
      const updatedHandymen = handymen.map(handyman => {
        if (handyman.id === handymanId) {
          return {
            ...handyman,
            reviews: [...handyman.reviews, reviewData]
          };
        }
        return handyman;
      });
      
      setHandymen(updatedHandymen);
      localStorage.setItem('handymen', JSON.stringify(updatedHandymen));
      
      const updatedUserReviews = [...userReviews, handymanId];
      setUserReviews(updatedUserReviews);
      localStorage.setItem('userReviews', JSON.stringify(updatedUserReviews));
    }
  };

  const handleLikeHandyman = (handymanId) => {
    if (!dislikedHandymen.includes(handymanId)) {
      const updatedHandymen = handymen.map(handyman => {
        if (handyman.id === handymanId) {
          const isLiked = likedHandymen.includes(handymanId);
          return {
            ...handyman,
            likes: isLiked ? handyman.likes - 1 : handyman.likes + 1
          };
        }
        return handyman;
      });
      
      const updatedLikedHandymen = likedHandymen.includes(handymanId)
        ? likedHandymen.filter(id => id !== handymanId)
        : [...likedHandymen, handymanId];
      
      setHandymen(updatedHandymen);
      setLikedHandymen(updatedLikedHandymen);
      localStorage.setItem('handymen', JSON.stringify(updatedHandymen));
      localStorage.setItem('likedHandymen', JSON.stringify(updatedLikedHandymen));
    }
  };

  const handleDislikeHandyman = (handymanId) => {
    if (!likedHandymen.includes(handymanId)) {
      const updatedHandymen = handymen.map(handyman => {
        if (handyman.id === handymanId) {
          const isDisliked = dislikedHandymen.includes(handymanId);
          return {
            ...handyman,
            dislikes: isDisliked ? handyman.dislikes - 1 : handyman.dislikes + 1
          };
        }
        return handyman;
      });
      
      const updatedDislikedHandymen = dislikedHandymen.includes(handymanId)
        ? dislikedHandymen.filter(id => id !== handymanId)
        : [...dislikedHandymen, handymanId];
      
      setHandymen(updatedHandymen);
      setDislikedHandymen(updatedDislikedHandymen);
      localStorage.setItem('handymen', JSON.stringify(updatedHandymen));
      localStorage.setItem('dislikedHandymen', JSON.stringify(updatedDislikedHandymen));
    }
  };

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <div className="max-w-5xl mx-auto">
        <h1 className="text-4xl font-bold text-gray-800 mb-8 text-center">
          Find Your Perfect Handyman
        </h1>
        
        <div className="bg-white rounded-lg shadow-lg p-6 mb-8">
          <div className="flex flex-col md:flex-row gap-4">
            <div className="flex-1 relative">
              <input
                type="search"
                value={searchQuery}
                onChange={(e) => setSearchQuery(e.target.value)}
                placeholder="Search by name"
                className="w-full pl-10 pr-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
              />
              <Search className="absolute left-3 top-2.5 text-gray-400" size={20} />
            </div>
            
            <select
              value={selectedSkill}
              onChange={(e) => setSelectedSkill(e.target.value)}
              className="w-full md:w-1/3 p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400"
            >
              <option value="">All skills</option>
              <option value="Plumbing">Plumbing</option>
              <option value="Electrical">Electrical</option>
              <option value="Carpentry">Carpentry</option>
              <option value="Painting">Painting</option>
            </select>
          </div>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {filteredHandymen.map((handyman) => (
            <div
              key={handyman.id}
              className="bg-white rounded-lg shadow-lg overflow-hidden transform transition-transform hover:scale-105"
            >
              <div className="p-6">
                <div className="flex justify-between items-start mb-4">
                  <div>
                    <h2 className="text-xl font-bold text-gray-800">{handyman.name}</h2>
                    <div className="flex items-center mt-1">
                      <Star className="text-yellow-400" size={16} />
                      <span className="ml-1 text-gray-600 text-sm">{handyman.rating}/5</span>
                    </div>
                  </div>
                  <button
                    onClick={() => setSelectedHandyman(handyman)}
                    className="bg-blue-100 text-blue-600 px-3 py-1 rounded-full text-sm hover:bg-blue-200"
                  >
                    View Profile
                  </button>
                </div>

                <div className="flex flex-wrap gap-2 mb-4">
                  {handyman.skills.map((skill, index) => (
                    <span
                      key={index}
                      className="bg-purple-100 text-purple-700 px-2 py-1 rounded-full text-xs"
                    >
                      {skill}
                    </span>
                  ))}
                </div>

                <div className="flex justify-between items-center mt-4">
                  <div className="flex items-center gap-4">
                    <button
                      onClick={() => handleLikeHandyman(handyman.id)}
                      className={`p-2 rounded-full transition-colors flex items-center gap-2 ${
                        likedHandymen.includes(handyman.id)
                          ? 'bg-green-100 text-green-600'
                          : 'hover:bg-gray-100 text-gray-500'
                      }`}
                      disabled={dislikedHandymen.includes(handyman.id)}
                    >
                      <ThumbsUp size={20} />
                      <span className="text-sm font-medium">{handyman.likes}</span>
                    </button>
                    <button
                      onClick={() => handleDislikeHandyman(handyman.id)}
                      className={`p-2 rounded-full transition-colors flex items-center gap-2 ${
                        dislikedHandymen.includes(handyman.id)
                          ? 'bg-red-100 text-red-600'
                          : 'hover:bg-gray-100 text-gray-500'
                      }`}
                      disabled={likedHandymen.includes(handyman.id)}
                    >
                      <ThumbsDown size={20} />
                      <span className="text-sm font-medium">{handyman.dislikes}</span>
                    </button>
                  </div>
                  <span className="text-sm text-gray-500">
                    {handyman.reviews.length} reviews
                  </span>
                </div>
              </div>
            </div>
          ))}
        </div>

        {selectedHandyman && (
          <HandymanModal
            handyman={selectedHandyman}
            onClose={() => setSelectedHandyman(null)}
            onAddReview={handleAddReview}
            hasUserReviewed={userReviews.includes(selectedHandyman.id)}
          />
        )}
      </div>
    </div>
  );
};

export default App;
