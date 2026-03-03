import React, { useState } from 'react';
import {
  View,
  Text,
  StyleSheet,
  ScrollView,
  TouchableOpacity,
  TextInput,
  Image,
  Alert,
  ActivityIndicator,
  Dimensions,
  Platform,
} from 'react-native';
import { LinearGradient } from 'expo-linear-gradient';
import { Ionicons } from '@expo/vector-icons';

const { width } = Dimensions.get('window');
const EXPO_PUBLIC_BACKEND_URL = process.env.EXPO_PUBLIC_BACKEND_URL;

interface ContactFormData {
  name: string;
  email: string;
  organization: string;
  message: string;
}

export default function Index() {
  const [activeSection, setActiveSection] = useState('home');
  const [formData, setFormData] = useState<ContactFormData>({
    name: '',
    email: '',
    organization: '',
    message: '',
  });
  const [isSubmitting, setIsSubmitting] = useState(false);

  const scrollViewRef = React.useRef<ScrollView>(null);

  const handleSubmitContact = async () => {
    // Validation
    if (!formData.name || !formData.email || !formData.message) {
      Alert.alert('Error', 'Please fill in all required fields');
      return;
    }

    // Email validation
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(formData.email)) {
      Alert.alert('Error', 'Please enter a valid email address');
      return;
    }

    setIsSubmitting(true);

    try {
      const response = await fetch(`${EXPO_PUBLIC_BACKEND_URL}/api/contact`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData),
      });

      if (response.ok) {
        Alert.alert(
          'Success',
          'Thank you for contacting Aurora Engineering! We will get back to you soon.',
          [
            {
              text: 'OK',
              onPress: () => {
                setFormData({
                  name: '',
                  email: '',
                  organization: '',
                  message: '',
                });
              },
            },
          ]
        );
      } else {
        Alert.alert('Error', 'Failed to submit form. Please try again.');
      }
    } catch (error) {
      console.error('Error submitting form:', error);
      Alert.alert('Error', 'Network error. Please check your connection.');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <View style={styles.container}>
      <ScrollView
        ref={scrollViewRef}
        style={styles.scrollView}
        showsVerticalScrollIndicator={false}
      >
        {/* Hero Section */}
        <View style={styles.heroSection}>
          <Image
            source={{ uri: 'https://images.unsplash.com/photo-1484600899469-230e8d1d59c0?crop=entropy&cs=srgb&fm=jpg&ixid=M3w3NTY2ODh8MHwxfHNlYXJjaHwxfHxzcGFjZWNyYWZ0fGVufDB8fHxibHVlfDE3NzI1MDU5NTJ8MA&ixlib=rb-4.1.0&q=85' }}
            style={styles.heroImage}
            resizeMode="cover"
          />
          <LinearGradient
            colors={['rgba(0,0,0,0.3)', 'rgba(0,0,30,0.95)']}
            style={styles.heroOverlay}
          >
            <View style={styles.heroContent}>
              <Text style={styles.heroTitle}>Aurora Engineering</Text>
              <Text style={styles.heroSubtitle}>
                From Simulation to Operation
              </Text>
              <Text style={styles.heroDescription}>
                Supporting clients from initial designs and simulations to flight
                operations with theoretical and experimental engineering skills.
              </Text>
            </View>
          </LinearGradient>
        </View>

        {/* Services Section */}
        <View style={styles.section}>
          <Text style={styles.sectionTitle}>Our Services</Text>
          <Text style={styles.sectionSubtitle}>
            Supporting Your Success Across All Phases
          </Text>

          {/* Service Card 1 */}
          <View style={styles.serviceCard}>
            <View style={styles.serviceIconContainer}>
              <Ionicons name="analytics" size={32} color="#4A90E2" />
            </View>
            <Text style={styles.serviceTitle}>
              Data Science, Modeling & Simulation
            </Text>
            <Text style={styles.serviceDescription}>
              • AI/ML Data Science & Recovery{`\n`}
              • Model Based Systems Engineering{`\n`}
              • Digital Twins & Simulation{`\n`}
              • Physics Informed AI/ML{`\n`}
              • System Aging & Lifetime Prediction
            </Text>
          </View>

          {/* Service Card 2 */}
          <View style={styles.serviceCard}>
            <View style={styles.serviceIconContainer}>
              <Ionicons name="rocket" size={32} color="#7B68EE" />
            </View>
            <Text style={styles.serviceTitle}>
              Spacecraft & Mission Operations
            </Text>
            <Text style={styles.serviceDescription}>
              • Multi-Spacecraft Trusted Autonomy{`\n`}
              • On-board Fault Detection & Recovery{`\n`}
              • Health and Safety Monitoring{`\n`}
              • Instrument Calibration{`\n`}
              • Science Operations Centers
            </Text>
          </View>

          {/* Service Card 3 */}
          <View style={styles.serviceCard}>
            <View style={styles.serviceIconContainer}>
              <Ionicons name="hardware-chip" size={32} color="#E24A90" />
            </View>
            <Text style={styles.serviceTitle}>Hardware Development</Text>
            <Text style={styles.serviceDescription}>
              • Space Flight Hardware{`\n`}
              • Board Layout and Design{`\n`}
              • Instrument Design, Build, Test{`\n`}
              • Cleanroom Operations{`\n`}
              • Lab Equipment Automation
            </Text>
          </View>

          {/* Service Card 4 */}
          <View style={styles.serviceCard}>
            <View style={styles.serviceIconContainer}>
              <Ionicons name="globe" size={32} color="#4AE290" />
            </View>
            <Text style={styles.serviceTitle}>
              Space Operations & Ground Support
            </Text>
            <Text style={styles.serviceDescription}>
              • Spaceflight Instrument Calibration{`\n`}
              • Commanding & Telemetry Monitoring{`\n`}
              • Data Handling and Processing{`\n`}
              • Data Correction & Distribution{`\n`}
              • Ground Support Systems
            </Text>
          </View>
        </View>

        {/* About Section */}
        <View style={styles.section}>
          <Text style={styles.sectionTitle}>About Aurora Engineering</Text>
          <Image
            source={{ uri: 'https://images.unsplash.com/photo-1657344956545-8f49e1b1f661?crop=entropy&cs=srgb&fm=jpg&ixid=M3w3NTY2ODh8MHwxfHNlYXJjaHwyfHxzcGFjZWNyYWZ0fGVufDB8fHxibHVlfDE3NzI1MDU5NTJ8MA&ixlib=rb-4.1.0&q=85' }}
            style={styles.aboutImage}
            resizeMode="cover"
          />
          <View style={styles.aboutCard}>
            <Text style={styles.aboutText}>
              Aurora Engineering provides expertise to a wide variety of
              governmental agencies including NASA and the Department of Defense.
            </Text>
            <Text style={styles.aboutText}>
              From theoretical modeling to spacecraft instrument check-out,
              Aurora's engineers support a diverse set of missions. We maintain
              world-class research facilities, including clean rooms and vacuum
              systems to NASA flight hardware standards.
            </Text>
            <View style={styles.highlightBox}>
              <Ionicons name="star" size={24} color="#FFD700" />
              <Text style={styles.highlightText}>
                Supporting missions from research facility management to leading
                day-to-day spacecraft instrument operations.
              </Text>
            </View>
          </View>
        </View>

        {/* Contact Section */}
        <View style={styles.section}>
          <Text style={styles.sectionTitle}>Contact Us</Text>
          <Text style={styles.sectionSubtitle}>
            Ready to discuss your project? Get in touch with our team.
          </Text>

          <View style={styles.contactForm}>
            <View style={styles.inputGroup}>
              <Text style={styles.inputLabel}>
                Name <Text style={styles.required}>*</Text>
              </Text>
              <TextInput
                style={styles.input}
                placeholder="Your name"
                placeholderTextColor="#888"
                value={formData.name}
                onChangeText={(text) =>
                  setFormData({ ...formData, name: text })
                }
              />
            </View>

            <View style={styles.inputGroup}>
              <Text style={styles.inputLabel}>
                Email <Text style={styles.required}>*</Text>
              </Text>
              <TextInput
                style={styles.input}
                placeholder="your.email@example.com"
                placeholderTextColor="#888"
                keyboardType="email-address"
                autoCapitalize="none"
                value={formData.email}
                onChangeText={(text) =>
                  setFormData({ ...formData, email: text })
                }
              />
            </View>

            <View style={styles.inputGroup}>
              <Text style={styles.inputLabel}>Organization</Text>
              <TextInput
                style={styles.input}
                placeholder="Your organization (optional)"
                placeholderTextColor="#888"
                value={formData.organization}
                onChangeText={(text) =>
                  setFormData({ ...formData, organization: text })
                }
              />
            </View>

            <View style={styles.inputGroup}>
              <Text style={styles.inputLabel}>
                Message <Text style={styles.required}>*</Text>
              </Text>
              <TextInput
                style={[styles.input, styles.textArea]}
                placeholder="Tell us about your project..."
                placeholderTextColor="#888"
                multiline
                numberOfLines={4}
                value={formData.message}
                onChangeText={(text) =>
                  setFormData({ ...formData, message: text })
                }
              />
            </View>

            <TouchableOpacity
              style={[
                styles.submitButton,
                isSubmitting && styles.submitButtonDisabled,
              ]}
              onPress={handleSubmitContact}
              disabled={isSubmitting}
            >
              {isSubmitting ? (
                <ActivityIndicator color="#fff" />
              ) : (
                <>
                  <Text style={styles.submitButtonText}>Send Message</Text>
                  <Ionicons name="send" size={20} color="#fff" />
                </>
              )}
            </TouchableOpacity>
          </View>
        </View>

        {/* Footer */}
        <View style={styles.footer}>
          <Text style={styles.footerText}>© 2025 Aurora Engineering</Text>
          <Text style={styles.footerSubtext}>
            From Simulation to Operation
          </Text>
        </View>
      </ScrollView>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#0a0a1a',
  },
  scrollView: {
    flex: 1,
  },
  heroSection: {
    height: 500,
    position: 'relative',
  },
  heroImage: {
    width: '100%',
    height: '100%',
  },
  heroOverlay: {
    position: 'absolute',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    justifyContent: 'center',
    alignItems: 'center',
  },
  heroContent: {
    alignItems: 'center',
    paddingHorizontal: 24,
  },
  heroTitle: {
    fontSize: 42,
    fontWeight: 'bold',
    color: '#fff',
    textAlign: 'center',
    marginBottom: 12,
    textShadow: '0px 2px 10px rgba(0, 0, 0, 0.75)',
  },
  heroSubtitle: {
    fontSize: 20,
    color: '#4A90E2',
    textAlign: 'center',
    marginBottom: 16,
    fontWeight: '600',
  },
  heroDescription: {
    fontSize: 16,
    color: '#ddd',
    textAlign: 'center',
    lineHeight: 24,
    maxWidth: 600,
  },
  section: {
    paddingHorizontal: 24,
    paddingVertical: 40,
  },
  sectionTitle: {
    fontSize: 32,
    fontWeight: 'bold',
    color: '#fff',
    marginBottom: 8,
    textAlign: 'center',
  },
  sectionSubtitle: {
    fontSize: 16,
    color: '#888',
    textAlign: 'center',
    marginBottom: 32,
  },
  serviceCard: {
    backgroundColor: 'rgba(30, 30, 50, 0.8)',
    borderRadius: 16,
    padding: 24,
    marginBottom: 20,
    borderWidth: 1,
    borderColor: 'rgba(74, 144, 226, 0.2)',
  },
  serviceIconContainer: {
    width: 64,
    height: 64,
    borderRadius: 32,
    backgroundColor: 'rgba(74, 144, 226, 0.1)',
    justifyContent: 'center',
    alignItems: 'center',
    marginBottom: 16,
  },
  serviceTitle: {
    fontSize: 22,
    fontWeight: 'bold',
    color: '#fff',
    marginBottom: 12,
  },
  serviceDescription: {
    fontSize: 15,
    color: '#bbb',
    lineHeight: 24,
  },
  aboutImage: {
    width: '100%',
    height: 200,
    borderRadius: 16,
    marginBottom: 24,
  },
  aboutCard: {
    backgroundColor: 'rgba(30, 30, 50, 0.6)',
    borderRadius: 16,
    padding: 24,
  },
  aboutText: {
    fontSize: 16,
    color: '#ddd',
    lineHeight: 26,
    marginBottom: 16,
  },
  highlightBox: {
    backgroundColor: 'rgba(74, 144, 226, 0.1)',
    borderLeftWidth: 4,
    borderLeftColor: '#4A90E2',
    padding: 16,
    borderRadius: 8,
    flexDirection: 'row',
    alignItems: 'center',
    marginTop: 8,
  },
  highlightText: {
    fontSize: 15,
    color: '#fff',
    marginLeft: 12,
    flex: 1,
    lineHeight: 22,
  },
  contactForm: {
    backgroundColor: 'rgba(30, 30, 50, 0.6)',
    borderRadius: 16,
    padding: 24,
    marginTop: 16,
  },
  inputGroup: {
    marginBottom: 20,
  },
  inputLabel: {
    fontSize: 16,
    color: '#fff',
    marginBottom: 8,
    fontWeight: '500',
  },
  required: {
    color: '#E24A90',
  },
  input: {
    backgroundColor: 'rgba(255, 255, 255, 0.05)',
    borderWidth: 1,
    borderColor: 'rgba(74, 144, 226, 0.3)',
    borderRadius: 8,
    padding: 16,
    fontSize: 16,
    color: '#fff',
  },
  textArea: {
    height: 120,
    textAlignVertical: 'top',
  },
  submitButton: {
    backgroundColor: '#4A90E2',
    borderRadius: 8,
    padding: 16,
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'center',
    marginTop: 8,
  },
  submitButtonDisabled: {
    backgroundColor: '#555',
  },
  submitButtonText: {
    color: '#fff',
    fontSize: 18,
    fontWeight: 'bold',
    marginRight: 8,
  },
  footer: {
    backgroundColor: 'rgba(10, 10, 26, 0.95)',
    padding: 32,
    alignItems: 'center',
    borderTopWidth: 1,
    borderTopColor: 'rgba(74, 144, 226, 0.2)',
  },
  footerText: {
    color: '#fff',
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 8,
  },
  footerSubtext: {
    color: '#888',
    fontSize: 14,
  },
});
🔧 Backend Code (/app/backend/server.py)
from fastapi import FastAPI, APIRouter, HTTPException
from dotenv import load_dotenv
from starlette.middleware.cors import CORSMiddleware
from motor.motor_asyncio import AsyncIOMotorClient
import os
import logging
from pathlib import Path
from pydantic import BaseModel, Field, EmailStr
from typing import Optional
import uuid
from datetime import datetime


ROOT_DIR = Path(__file__).parent
load_dotenv(ROOT_DIR / '.env')

# MongoDB connection
mongo_url = os.environ['MONGO_URL']
client = AsyncIOMotorClient(mongo_url)
db = client[os.environ['DB_NAME']]

# Create the main app without a prefix
app = FastAPI()

# Create a router with the /api prefix
api_router = APIRouter(prefix="/api")


# Define Models
class ContactInquiry(BaseModel):
    id: str = Field(default_factory=lambda: str(uuid.uuid4()))
    name: str
    email: EmailStr
    organization: Optional[str] = None
    message: str
    timestamp: datetime = Field(default_factory=datetime.utcnow)
    status: str = "new"  # new, contacted, resolved

class ContactInquiryCreate(BaseModel):
    name: str
    email: EmailStr
    organization: Optional[str] = None
    message: str

# Add your routes to the router instead of directly to app
@api_router.get("/")
async def root():
    return {"message": "Aurora Engineering API"}

@api_router.post("/contact", response_model=ContactInquiry)
async def create_contact_inquiry(input: ContactInquiryCreate):
    """
    Submit a contact inquiry from the Aurora Engineering website
    """
    try:
        # Create the inquiry object
        inquiry_dict = input.dict()
        inquiry_obj = ContactInquiry(**inquiry_dict)
        
        # Store in MongoDB
        result = await db.contact_inquiries.insert_one(inquiry_obj.dict())
        
        if result.inserted_id:
            logger.info(f"New contact inquiry from {inquiry_obj.name} ({inquiry_obj.email})")
            return inquiry_obj
        else:
            raise HTTPException(status_code=500, detail="Failed to save inquiry")
    except Exception as e:
        logger.error(f"Error creating contact inquiry: {str(e)}")
        raise HTTPException(status_code=500, detail=str(e))

@api_router.get("/contact/inquiries")
async def get_contact_inquiries(limit: int = 50, status: Optional[str] = None):
    """
    Get all contact inquiries (admin endpoint)
    """
    try:
        query = {}
        if status:
            query["status"] = status
        
        inquiries = await db.contact_inquiries.find(query).sort("timestamp", -1).limit(limit).to_list(limit)
        
        # Convert MongoDB documents to JSON-serializable format
        for inquiry in inquiries:
            if "_id" in inquiry:
                inquiry["_id"] = str(inquiry["_id"])
        
        return inquiries
    except Exception as e:
        logger.error(f"Error fetching inquiries: {str(e)}")
        raise HTTPException(status_code=500, detail=str(e))

# Include the router in the main app
app.include_router(api_router)

app.add_middleware(
    CORSMiddleware,
    allow_credentials=True,
    allow_origins=["*"],
    allow_methods=["*"],
    allow_headers=["*"],
)

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

@app.on_event("shutdown")
async def shutdown_db_client():
    client.close()
